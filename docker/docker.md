# A docker summary (a quick review of Docker ?)

TL;DR
I used Docker during the last months. I write down here what I learnt from it, what you can do with it and some tricks you need to be aware of.

## What is docker ?
### As a User
[Docker](https://github.com/moby/moby) is a project developped in Go. You can see it as a tool to put your apps/code/database/services in containers. As a user, containers look like virtual machines you can start and stop quickly. In fact, you can when your **image** is build, starting/stoping docker containers should take only some seconds.

### As a sysadmin
Docker containers are based on [LXC](https://en.wikipedia.org/wiki/LXC)

In fact there is no hypervisor like with a VM. The term "lightweigt virtualization" is often used. The size of a docker image can go from some MB to some hundred of MB. The average size is way less than with a VM. In fact, it takes less CPU/RAM/Disk than a disk, that is why for the same bare-metal machine, you can get more with docker than with VMs. It's like filling a box with rocks or with sand.
![](https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/docker/docker_vm.png) 

## Why is it cool ?
Docker begins to be widely used today, by dev, devops, Cloud Provider etc. Why ?

- **Easy deployment**: Each container has a single configuration file, the Dockerfile, where all the instructions to create the image are located. You want to launch your app ? Easy!
- **Same prod and dev** (if you want): You can have the same environment for the production ans the development, because each part of your project is in a container, which is **isolated**. No more issues like "it worked on my computer!", it is easier to reproduce a bug.
- **Creating micro-services architecture**: When you want your project to scale up, you have several possibilities for the service, like horizontal scalability. But maybe you also want your code to be better organized, so to maintain it, you can develop a [micro-services architecture](https://en.wikipedia.org/wiki/Microservices). Docker is a very good choice for this approach, whether you use [docker-compose](https://github.com/docker/compose) when deploying your services when it is a small project, or [Kubernetes](https://github.com/kubernetes/kubernetes) for a more robust solution.

- It is **plateform-agnostic**, you can run it on Windows, Linux (Debian, Ubuntu, CentOS ...), on AWS etc.
- You can access to **docker images** of the community freely. You want to deploy an nginx ? Just pull the official image, and use it
- Another use case is to use it to **execute your tests**. It is isolated, so you can install whatever you need, no problem! 
- You can **duplicate containers** to have horizontal scalability (more advanced use case).

## How to use it ?
Here is the process of building a *container*
![](https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/docker/docker_build.png) 

At the beginning, you write a **Dockerfile** to describe what you want to put in your **image**: a python project? A java? A PostgreSQL database ? Then, you can create an **image**, from this dockerfile. From this image, you can run one or more **containers**. What is the difference between images and containers ? You can see it like in OOP: the image is the class, and the container is the object. You can instantiate several objects from a single class, each of them owning their data. If we continue with the metaphor we can say that the Dockerfile corresponds to the code you write to describe you class.

### Docker pull
On the schema, you also have the **docker hub**, wich is a place when you can push you images, and where you can also pull images from other people !
For example, if I want to run a nginx locally, I can pull the image locally like this:

``` bash
docker pull nginx
```
Then, you can instantiate it like this (=creating a container):

``` bash
docker run -p 80:80 nginx
```
NB: The -p option is here to map the port of the host to the port of the container. Each request coming on the port 80 of the host will be redirect to the port 80 of the container.

### Docker build
Maybe you want to create your own image. Well for that, you have to write your configuration in a Dockerfile (written that way)
Let's say we want to create an hello-world with python.

``` bash
mkdir hello-world && cd hello-world
``` 

We create a python file wich print our message:
``` python

```


``` Dockerfile
FROM python:3.6


```


## Be stateless !


## Some tips/problem encoutered
- docker-compose probleme quand up des nouveaux containers
- ajouter le contexte au build (issue github)
- le stateful est source de bcp erreurs
- expose, publish and -p
- entrypoint vs cmd
- cmd faut mettre des "
- port un seul par container
