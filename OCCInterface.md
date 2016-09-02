# OCCInterface

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
The aim of OCCInterface is to provide a simple interface with your OCCI server. You can use it as a tool to manage resources, 
explore them, documentate and reproduce bugs to communicate with your team.

Here is a list of feature:
- explore the by making GET,POST,PUT and DELETE
- click on 

### Technical stack
I developped this app with ReactJS (with Redux, react-router), webpack to compile/minify/dev, NodeJS and express for the server. I also
used travis CI on heroku, and some bash to make it properly works.

