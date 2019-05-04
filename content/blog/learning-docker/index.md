---
title: Learning Docker and Cycle.io
date: 2019-05-04
---

# Learning Docker and Cycle

Docker has taken the world over by a storm (or at least, taken over my world by a storm, the rest of the world realized its importance a while back). But what exactly is Docker? I recently got a chance to learn Docker on my own, and this post documents my experience on what I learned, and aims to be a bird's eye view on Container technology for those who do not have the faintest idea what that is (just like me, when I started). This post will not teach you how to make a Docker container (there are far better resources out there for that, made by far more knowledgable people, I list some of those at the end), but it will teach you (I hope) what Docker is, and will act as a building block for you to learn further. 


## What is Docker

Docker is a software, that helps us run other softwares by making Images and Containers. Images and Containers are a self contained unit of software, with all it's dependencies included. Go have a look at the Docker's Logo, it is a whale carrying shipping containers, and that is exactly how Docker works. It transforms all your project files, folders and other stuff into these "shipping containers", each self-sufficient.

So it is sort of like a plug-and-play system, you just download a "box", plug it into Docker, and off it runs, without a care in the world what operating system your computer has, or whether any dependency is installed on your system at all.

Lets carry this analogy further, lets just say that you imported your stuff in a shipping container, and now you have to place the container in your backyard. The problem is, that container is either way too bif or way too small for your home. What docker does is it makes those containers standardized, so regardless of how large or small your backyard is, all shipping containers "fit" there.

One thing that may seem difficult at first is a plethora of docker commands, but once you start building a few containers, they become second nature. Most useful and oft-used commands are `docker build ...` `docker run ...`, `docker ps` `docker image ps` `docker image ls` etc...

Another important bit is the Dockerfile, the file that the Docker software uses to create Images. A sample Docker file might look like this

```

```

Technically, Docker does not make Containers out of your project directly, it makes "Images", and yeah as the name suggests, Images are what Docker containers come out of. You download an Image, make a Container out of it, and run your app as that container. There are hundreds, maybe thousands of Images on Docker Hub, a central repository for docker Images. It usually takes some time the first time you make an Image, but after that, Docker is smart enough to cache your build, and on next build command read your dependency list from your manifest files, and if unchanged, just used the cached version. 

WHat this means in terms of software is that whatever your operating system be, a docker container can run inside of it, even if that container was assembled in Windows, Linux or Mac. All we need is Docker software itself installed on our systems.

What does this achieve you ask? Why containerize my applications? Containerization has many benefits.
<!-- 
Containers offer a packaging mechanism in which applications can be abstracted from the environment in which they actually run. This decoupling allows container-based applications to be deployed easily and consistently, regardless of the nature of target environment. This gives developers the ability to create predictable environments that are isolated from rest of the applications and can be run anywhere.

Docker is a tool that is designed to benefit both developers and system administrators, making it a part of many DevOps (developers + operations) toolchains. For developers, it means that they can focus on writing code without worrying about the system that it will ultimately be running on. It also allows them to get a head start by using one of thousands of programs already designed to run in a Docker container as a part of their application. For operations staff, Docker gives flexibility and potentially reduces the number of systems needed because of its small footprint and lower overhead. -->

  - It helps you avoid dependency issues such as clashes between dependencies, dependency variations among operating systems, one program changing the version of a dependency, which renders another program useless, and stuff like that.
  - It takes the "Module" model of software development to the level of app deployment, you can use Images that other people have used and integrate them seamlessly into your own apps, without any worry.
  - One of the biggest advantages to a Docker-based architecture is actually standardization. Docker provides repeatable development, build, test, and production environments. Standardizing service infrastructure across the entire pipeline allows every team member to work in a production parity environment. 
  - Docker containers allow you to commit changes to your Docker images and version control them. For example, if you perform a component upgrade that breaks your whole environment, it is very easy to rollback to a previous version of your Docker image. This whole process can be tested in a few minutes.
  - Eliminate the “it works on my machine” problem once and for all. One of the benefits that the entire team will appreciate is parity. Parity, in terms of Docker, means that your images run the same no matter which server or whose laptop they are running on.
  - Docker manages to reduce deployment to seconds. This is due to the fact that it creates a container for every process and does not boot an OS.
  - Docker ensures your applications and resources are isolated and segregated. Docker makes sure each container has its own resources that are isolated from other containers. You can have various containers for separate applications running completely different stacks. Docker helps you ensure clean app removal since each application runs on its own container. If you no longer need an application, you can simply delete its container. It won’t leave any temporary or configuration files on your host OS. 
  - Docker also ensures that each application only uses resources that have been assigned to them.
  - The last of these benefits of using docker is security. From a security point of view, Docker ensures that applications that are running on containers are completely segregated and isolated from each other, granting you complete control over traffic flow and management. No Docker container can look into processes running inside another container.

## Deployment

So, you have containerized your applications, how do you deploy it?

One way to deploy your containers is to deploy them on a cloud server (a computer running on, well, cloud), monitered by a Container orchestration system. One of the best known container orchestration system is Kubernetes, another is Cycle, the one I decided to use.

Cycle claims to be a container orchestration system that promises to deliver, easing the process of contatiner management for you by letting you focus solely on the code, and not on infrastructure. So, it offers you a dashboard, from where you can easily control how your containers are deployed. It currently partners with Vultr, a cloud service provider, a company that hosts computers for you on which host your application. Signing up and setting up a server with Vultr was a breeze, and same was the case for Cycle. Mind you, both these services are not free, but, Vultr provides free credit for acts like following them on twitter, and Cycle provides $25 credits for the first month. That is enough bang to try out these services.

<!-- There are many services who offer cloud servers, Google's Cloud Platform, AWS, Docker itself, Microsoft Azure, iBM Cloud, SalesForce, DropBox etc.

So I finished my application, writing a docker-compose file to run them simultaneously (what a feeling it was when my docker-compose finally ran!). Now I wanted to deploy it (For free, preferably). I usually deploy either on netlify, or on Heroku. But I needed cloud service provided.

I tried a lot of different options, but as a front-end developer who is finding it hard to cope with CSS (darn you CSS, why you so...hard), I found alot of daunting terminolgy, each service provided using their own jumble of words to confuse the heck out of you. So I gave up on a lot of well known options, until I found cycle.io. -->

Deploying a server, and then deploying my app through Cycle on that server, proved to be a really easy task. You set up a 
1. cloud service provider, currently (Vultr and Packet, soon AWS will be available too). 
2. then select a server size (called infrastructure), based on your needs and budget
3. Upload your Docker Image.
4. Make Stacks out of your docker Image (a stack is group of images; a snapshot of image builds; atomic units, a bit like Images themselves), specified by a `cycle.json` file, called a stack file (more on this below).
5. Deploy your Stacks to an Environment, which automatically builds a private, encrypted network on which the containers communicate.

The only challenging aspect of the whole process is the creation of `cycle.json` file, however, if you have experience with how `docker-compse.yml` file works, you can easily figure out how to make the cycle file. In the future, you will be able to generate `cycle.json` file automatically from a `docker-compose.yml` file.

From each Environment's dashboard, one can easily start, stop or restart containers as they wish.
Each Environment comes with services like VPN (Virtual Private Network), Load balancer, and Discovery (a DNS service) 