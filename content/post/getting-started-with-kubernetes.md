---
title: Getting Started with Kubernetes
subtitle: All you need to know about Kubernetes to get started
date: 2020-10-02T23:00:00+00:00
tags: []
draft: true

---
## Why Kubernetes

Before we dive into the lingua specifics of Kubernetes, let's get an idea of why this is important and everyone is talking about it.

### Application Deployment on a Virtual Machine

Not so long ago, the default way to deploy an app was on a physical server. To set one up, you define some physical space, power, cooling, network connectivity for it, then install an operating system and any software dependencies, and then finally the software itself. Whenever you need more processing power, redundancy, security, scalability for it, what do you do? You guess right, you would have to simply add more computers. It was very common for each computer to have a single purpose, for example, database, web server, or content delivery. So you begin to see the problem with doing thins like this. It wasted a lot of resources, and it took a lot of time to maintain and scale. It also wasn’t portable at all, applications were built for specific hardware and operating systems as well.

![](https://lh4.googleusercontent.com/LsHHoRp0jGYNdZKj0aD2vu1d74Cki_JVgPjuPy6Yb7siHmTBTpxeJ_0K4e6VrF5afHYgKqHj_QG2N5VwtGbiGNnha88t9TvyLOTzaJ0cwoNiESn6y12otECRGJyRj-GugsrBiL0S =593x592)

Fax forward later, in comes the dawn of virtualization. Virtualization helped by making it possible to run multiple virtual servers and operating system on the same physical computer

#### Hypervisor:

A hypervisor is a software layer that brakes the dependencies of an operating system with its underline hardware. It allows several virtual machines to share that hardware. KVM(Kernel-based Virtual Machine) and Hyperver-v are well-known hypervisors.

Today you can use virtualization deploy and use servers fairly quickly. Adopting virtualization means it takes less time to deploy new resources, and we waste fewer resources than the physical computers were using and we get some improved portability because virtual machines can be imaged and moved around. However, the application and all its dependencies are still bundled together, and it’s not easy to move from a VM, from one hypervisor to another. Running multiple applications within a single VM also creates multiple problems, applications that share dependencies are not isolated from each other. A dependency upgrade for one application might cause the other to stop working. You can try to solve this problem with regular software engineering policies. For example, you can lock down the dependencies that no other applications are allowed to make changes but this leads to new problems because dependencies do need to be upgraded occasionally.

The VM centric way to solve this problem is to run a dedicated virtual machine for each application, each application maintains it’s own dependencies, and the kernel is isolated so one application won’t affect the performance of another. Here too we can run into issues as you would expect, scale this approach to 100 thousands of applications and you can quickly see the limitations.

![](https://lh5.googleusercontent.com/aQsU90gWxnHkKwk0_oIOiNMdYCyB2wuc5i0kfNifoJWKFWJv_2TAPyZkiw0KHeTgf4tlZBUKilByJr-kVxWscvNugtso0KgyxSLfi9C69wa52DJu0vA-c6dJZU8vsAzc-gPCwHhf =620x578)

A more efficient way to solve the dependency problem is to implement abstraction at the layer of the application and its dependencies. You don’t have to virtualize the entire machine or its operating system, but just the userspace. Userspace is all the code that resides above the Kernel and includes all the applications and their dependencies. This is what is it means to create a container. Containers in this context are isolated userspace for running application code. Containers are light-weight because they don’t carry a full operating system they can be scheduled or packed tightly onto the underlying system. Containerization is the next step in the evolution of managing code.

#### What are then containers?

refer to this [article](https://cybercog.co/post/from-containers-to-kubernetes-an-all-in-one-beginners-guide-part-1/) for a detailed understanding of containers. 

Let’s say your organization has embraced containers, but managing containers at scale is a tough challenge. Because containers are so lean, they can be created in numbers, far exceeding the counts of VMs that your organization used to have and the applications running in them need to communicate over the network but you don’t have a network fabric that lets containers find each other, you need help. How can you manage your container infrastructure better?

Kubernetes is an open-source platform that helps you manage your container infrastructure on-premise or in the cloud.

## Kubernetes for Grown-Ups

Kubernetes is a container-centric orchestration environment that automates the deployment, scaling, load balancing, logging, monitoring, and other management features of containerized applications. Other examples of container orchestration platforms include Docker-swam, Red-Hat openshift container platform, salt-stack, etc.