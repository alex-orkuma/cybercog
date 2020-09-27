---
title: From Containers to Kubernetes an all in One Beginners Guide Part 1
subtitle: Containers
date: 2020-09-26T23:00:00+00:00
tags:
- LXC
- LXD
- 'Kubernetes '
- 'Containers '
- Docker

---

Before now, there existed hardware virtualization technology which made running virtual machines possible, this solved a lot of problems but it had its challenges. It wasted resources and all that. we won't get too much into it. To solve some of those challenges, containers entered the mix.  
![](https://cdn.hashnode.com/res/hashnode/image/upload/v1601235648586/WchQK8YIf.jpeg)

### What exactly are containers?

Containers have existed within operating systems for quite a long time. A container is the runtime instantiation of a **Container Image**. A container is a standard Linux process typically created through a clone() system call instead of fork() or exec(). Also, containers are often isolated further through the use of cgroups, SELinux or AppArmor.

###  Container Runtime

A container runtime a lower-level component typically used in a **Container Engine** but can also be used by hand for testing. It is responsible for the following.

\- Consuming the container mount point provided by the Container Engine

\- Consuming the container metadata provided by the Container Engine (can be an also be a manually crafted config.json for testing)

\- Communicating with the kernel to start containerized processes (clone system call)

Among other things.

some of the most popular container runtime include:

\-  [runc](https://github.com/opencontainers/runc): Docker and so many popular container engines rely on runc

\-  crun

\-  [railcar](https://github.com/oracle/railcar)

\-  [katacontainers](https://katacontainers.io/)

### Container Engines

We have been talking about this for a while now what exactly it is?

A container engine is a piece of software that accepts user requests, including command-line options, pulls container images, and from the end user’s perspective runs the container. There are many container engines, including docker, RKT, CRI-O, and LXD. Also, many cloud providers, Platforms as a Service (PaaS), and Container Platforms have their own built-in container engines which consume Docker or OCI compliant Container Images. Having an industry-standard Container Image Format allows interoperability between all of these different platforms.

Going one layer deeper, most container engines don’t actually run the containers, they rely on an OCI compliant runtime like runc.

###  Container Image

Yeah I know it was going to be just a matter of time before we talked about container images.

A container image, in its simplest definition, is a file which is pulled down from a **Registry Server** and used locally as a mount point when starting Containers.   The container community uses “container image” quite a bit, but this nomenclature can be quite confusing. Container engines such as **Docker**, **RKT**, and even **LXD**, operate on the concept of pulling remote files and running them as a Container. Each of these technologies treats container images in different ways. we will talk more about this when we get to the docker section. Just put a container is a running instance of a container image.

###  Container Use Cases

There are many types of Container design patterns forming. Since containers are the run time version of a container image, the way it is built is tightly coupled to how it is run.

The common use cases of containers include:

####  Application Containers

Application containers are the most popular form of container. These are what developers and application owner’s care about. Probably what you will care about and a lot of definitions of containers online is tailored to this use case. Application containers contain the code that developers work on. They also include things like MySQL, Apache, MongoDB, and or Node.js. Container engines like Docker run Application containers

####  Operating System Containers

Operating System Containers are containers which are treated more like a full virtual operating system. Operating System Containers still share a host kernel but run a full init system which allows them to easily run multiple processes. LXC and LXD are examples of Operating System Containers because they are treated much like a full virtual machine. Docker Engines like LXD run operating system containers.

It is also possible to approximate an Operating System Container with docker/OCI based containers but requires running systemd inside the container.

###  Types of Container Images

Like we said earlier, the container is a running instance of an image os it's important to lay a distinction to the type of images that exist.

####  Base or Parent Images

Simply put, a base image is an image that has no parent layer. Typically, a base image contains a fresh copy of an operating system. Base images normally include the tools (yum, rpm, apt-get, dnf, microdnf) necessary to install packages / make updates to the image over time. While base images can be “handcrafted”, in practice they are typically produced and published by open source projects (like Debian, Fedora or CentOS) and vendors (like Red Hat)

####  Intermediate Images

An Intermediate image is any container image that relies on a base image. Typically, core builds, middleware and language runtimes are built as layers on “top of” a base image. These images are then referenced in the FROM directive of another image. These images are not used on their own, they are typically used as a building block to build a standalone image.

####  Application Images

These images are what end users consume. Use cases range from databases and web servers to applications and services buses.

The application image is compiled from file system layers built onto an intermediate image. Example of application image is your custom-built application image either for production or testing.

##  Docker

Now that we have gone over most of the terminologies, how does docker come into the mix?

Docker is a **container engine** that is based on the **runc **container runtime. In simple terms, Docker packages your application code into images and then run them as containers. These images can run on any system or server that runs the Docker daemon. You can begin to see the advantage of this. Basically, you don't have to worry about OS-level configurations and dependencies as you would when using Virtual Machines.

###  Docker Images  and Docker Containers

what exactly is a docker image? A Docker image is a file (called Dockerfile), comprised of multiple layers, that is used to execute code in a Docker container. A running instance of a docker image is called a docker container.

A typical example of a docker image looks like this

    FROM node:10.15.2-alpine

    WORKDIR /usr/src/app

    COPY package.json ./

    COPY .babelrc ./

    RUN npm install

    COPY --from=appbuild /usr/src/app/dist ./dist

    EXPOSE 4002

    CMD npm start

From the Dockerfile above you can see the different type of images will discuss above. This whole Dockerfile will be built into an **application image**. The **node 10.15.2** is the **intermediate image** based on an **alpine parent or base image**.

when this image is built and run it becomes a container.

In the next article, we will talk about what each line in this file does container orchestration and various orchestration tools like Kubernetes docker swam etc. We will then do a deep dive into Kubernetes.