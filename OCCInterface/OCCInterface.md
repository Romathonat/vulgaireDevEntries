
During my intership at Smile-OpenWide this summer, one my main project was the development of an application which aim is
to explore, documentate and vulgarize OCCI servers.

## What is an OCCI server ?
OCCI is a set of specifications, which aim is to normalize and unify the Cloud. Indeed, If you use the Cloud today and want 
to change your Cloud provider, it is really painful because the way of managing is different depending on the provider
you use. OCCI try to solve this issue.

In this standard, everything inherits from Resource or Link. For instance, an PostgreSQL database is a Resource, a Compute 
is a resource, and you can link them through an StorageLink.

The workflow is as following: you deploy an OCCI server, which expose a REST API, onto your Cloud, then you use this API to create
delete, modify resources and links easily.

## The OCCInterface
### What is it ?
![https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/OCCInterface/images/screenOccinterface.png](https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/OCCInterface/images/screenOccinterface.png)

The aim of OCCInterface is to provide a simple interface with your OCCI server. You can use it as a tool to manage resources, 
explore them, documentate and reproduce bugs to communicate with your team.

Here is a list of features:
- Use the OCCI server by making GET,POST,PUT and DELETE request.
- Pretty formatted and highlighted JSON
- Travel through resources referencing each other by symply clicking the link
- Select the server you are targeting (in non-integrated version)
- Write documentation with markdown
- Write playground links into the markdown, wich make a GET request when clicking on it
- Write sample links into the markdown, which make a POST and/or DELETE request when clicking on it. It is particulary useful to communicate with your team, instead of making a curl, just write the request in the markdown. 
- user friendly and quite sexy.

You can try it here : http://occinterface.herokuapp.com (make take a little time if the app is asleep)


### Technical stack
I developped this app with ReactJS (with Redux, react-router), webpack to compile/minify/dev, NodeJS and express for the server. I also
used travis CI on heroku, and some bash to make it properly works.

## Going further : ExploREST
In fact, 90% of the developement is generic, and can be used to make this application works with every REST API you want!  

It is a really cool tool to documentate you api, by puting sample/playground links to show simply how you API works to your user. Swagger is a reference in this domain, but here we have some interesting functionalities swagger does not have : playground links, sample links, markdown redaction, simplicity of use, cross resources browsing, a beautiful look.

This project is open-sourced, so do not hesitate to contribute (give advices, report bug or make pull requests)! [https://github.com/Romathonat/ExploREST](https://github.com/Romathonat/ExploREST)


