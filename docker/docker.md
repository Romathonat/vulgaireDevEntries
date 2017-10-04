# A docker summary

TL;DR
I used Docker during the last months. I write down here what I learnt from it, what you can do with it and some tricks you need to be aware of.

## What is docker ?
### As a User
[Docker](https://github.com/moby/moby) is a project developped in Go. You can see it as a tool to put your apps/code/database/services in containers. As a user, containers look like virtual machines you can start and stop quickly. In fact, you can when your **image** is build, starting/stoping docker containers should take only some seconds.

### As a sysadmin
Docker containers are based on [LXC](https://en.wikipedia.org/wiki/LXC)

In fact there is no hypervisor like with a VM. The term "lightweigt virtualization" is often used. The size of a docker image can go from some MB to some hundred of MB. The average size is way less than with a VM, that is why you can 
![](https://raw.githubusercontent.com/Romathonat/vulgaireDevEntries/master/docker/docker_vm.png) 

## Why is it cool ?
