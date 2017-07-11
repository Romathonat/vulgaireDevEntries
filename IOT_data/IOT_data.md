## Intro

We are using [timescaledb](https://www.timescale.com/), a plugin for postgreSQL, wich let you store a lot of time-serie data. 
There are several advantages with this solution, like the ability to use classical SQL, the fact its utilization is transparent, using postgres wich is a well-tested DBMS,
good performances, the power of relationnal databases... The project is open-source, and the team often delivers new version. I also discussed a little with them, they are reactive when you have a problem, and they seem to be nice guys :)

## Problem

There are data from multiples sites, each one with its own database. There is also a backend (let's say a cloud plateform), where all data from all sites are stored. How can we synchronize data each X minutes from each site to the backend, knowing that some data may be updated ?

## Solution

First, we need to maker sure there is no collision between data from different site, which means we need to have unique primary keys on all data. To do so, we can compose primary keys with the name of the local site, wich needs to be unique.

Then, we dump data to sql (insert into) format, on each site, then send it to the backend. On the backend, we execute each line one by one. If there is an Integrity error, it means we are trying to insert a key that is already present, so we do not need to insert the data but to update it. Indeed, the data may have been updated on site.

In practise, we use python for our backend, with psycopg2 to communicate with our postgreSQL, which is boosted for timeseries data with timescaleDB.

We have a function that transform an INSERT to an UPDATE (by @lucaslandry)

``` python
def insert_to_update(test_string, id_name):
    regex = "INSERT INTO (?P<table>[\w]+) \((?P<columns>.+)\) VALUES \((?P<values>.+)\)"
    match = re.search(regex, test_string)
    
    if match is not None:
        columns = match.group('columns').replace(' ', '').split(',')
        values =  match.group('values').replace(' ', '').split(',')

        update_command = "UPDATE {} SET ({}) = ({}) WHERE {} = {} ;".format(
                match.group('table'),
                match.group('columns'),
                match.group('values'),
                id_name, 
                values[columns.index(id_name)])
            
    
    return update_command
```

and another function that uses the psycopg2 error to get the value and name of the key with error:

``` python
import re 

def extract_id(error_string):
    regex = ("DETAIL:\s{2}Key\s\((.+)\)=\((.+)\)\salready\sexists")                     
    match = re.search(regex, error_string) 
    id_name, id_value = match.group(1), match.group(2)
    return id_name, id_value
```

And then we use them that way:

``` python
    # add your connection credentials
    con = psycopg2.connect(...)
    cur = con.cursor()
    print("Connection to Postgres Database established.")

    filename, extension = os.path.splitext(os.path.basename(body))

    with open(body, "r") as my_file:
        for line in my_file:
            if 'INSERT INTO' in line[:11]:
                try:
                    # we try to execute the insert
                    cur.execute(line)                
                    con.commit()
                except psycopg2.IntegrityError as e:
                    # if it does not work, we change that inser into to update.
                    # first we rollback the error transaction
                    con.rollback()

                    id_name, id_value = extract_id(e.pgerror)

                    update_command = insert_to_update(line, id_name)

                    cur.execute(update_command)
                    con.commit()

    con.close()
```

This approach works fine.

Issue: Updating each row when there is a conflict can be quite inefficient when working on hypertables, because these data does not need to be updated (sensor data in our case).
Solution: Differentiate each case, when working or not on an hypertable.
