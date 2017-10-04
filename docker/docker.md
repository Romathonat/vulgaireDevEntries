# A docker summary

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
