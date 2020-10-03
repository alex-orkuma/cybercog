---
title: Getting Started with Kubernetes
subtitle: All you need to know about Kubernetes to get started
date: 2020-10-02T23:00:00+00:00
tags: []

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

**![](https://lh6.googleusercontent.com/W9HE0nGM7PHGMZAyTnyRXZDjG8VR0lDksx5q9Ti-H0SQj8QiGaoscI4xKbJGJwMY0jnGc_bCAunsmP4qsqe5p2oHhtodrZhyGDXRxtUtqlVwahav8FpBBtn-3mth16xrv4ZcHnt7 =624x451)**

### **Kubernetes Concept**

To understand how Kubernetes works, there are two related concepts you need to understand. The first is the Kubernetes object model. Each thing Kubernetes manages is represented by an object. The second is the principle of declarative management, Kubernetes expects you to tell it what state you want the state of the objects under its management to be, it will work to bring that state into being and keep it there.

Kubernetes objects have two important elements,

#### Object-Spec

This is where the desired state of the object is described.

#### Object-Status

This is simply the current state of the object provided by the Kubernetes control plane.

![](https://lh5.googleusercontent.com/dFj14jkV39BlNVLnr5hCko5JPxw4t9xRYqwUQU_AIOHBbdec9CWPFblFkcrY7nYq3BnT0FqbS8fcQK6tRPVE3xydezYDW6Ilb-_pcAovk2-YRccqAoXZNs_PW3xCmjQn4_TghlYa =624x324)

### PODs

Pods are the basic building block of the standard Kubernetes model and they are the smallest deployable Kubernetes object. Every running container in a Kubernetes system is wrapped in a pod, a pod can accommodate one or more containers. If there is more than one container in a pod, they share resources including networking and storage (tight coupling). Kubernetes assigns each pod a unique IP address, every container in a Pod shares the network namespace including the IP and network main ports. Containers within the same Pods can communicate through the localhost. Finally, Pods can share storage with one another.

![](https://lh5.googleusercontent.com/YjPGtqSdpGioZevaS2hzOP4g4rMJkM8kJuuvH9sE2izRqB_q1jrlrv6JDFnF2YuEf4BcR1n0_x_0csVm0xoZ4kRfFELHZIOoeP48wgQ79CRJWjk2RAMPzJiM21YcIfdJQN2c_SUd =614x488)

#### Example:

Let’s consider a simple example where you want three instances of the Nginx server each in its container running all the time. This can be achieved by declaring some objects to represent those Nginx containers. What kind or type of object? Perhaps Pods. now it is Kubernetes’ job to lunch those containers and maintain them. The caveat to using Pods is, they are not self-healing so if we want to keep all the Nginx servers in existence and also working together as a team, we might want to ask for them using a more sophisticated kind of object called a deployment. We talk about a deployment later on.

Kubernetes will approach this by comparing the desired state which we have declared to the current state, if there is a mismatch it will then lunch the resources (in this case the Pods) to bring the state into the desired state. The Kubernetes control plane will continuously monitor the state of the cluster, endlessly comparing the state of the cluster to what has been declared.

  
![](https://lh3.googleusercontent.com/V9kcje7d7j6QZCINYwMXl6qXSRJwCXiAusuCGmVFScCx_RJzhCJ917PBsD2nwMSMJD-47T3ll_nCq53pBnN9BrZSN0LTmv-A078bEkTn-Pm5_XvrVrIRqUKsSQCE5oWdnkJInSkQ =624x468)

### Kubernetes Cluster

A Kubernetes cluster is made up of one computer usually a virtual machine called the master(**Control Plane**) and the other computers also virtual machines called a node. The job of the nodes is to run pods, the job of the master is to coordinate the entire cluster.

### Kubernetes Control Plane(master)

Several Kubernetes components run on the master(**control-plane**), the single component that you will interact with is the Kube API server.

#### **Kube API server.**

This component’s job is to accept commands that view or change the state of the cluster, including launching pods. The kubectl command’s job is to connect to the Kube API server and communicate with it using the Kubernetes API.

Kube API server also authenticates the incoming request, determines whether they are authorized and valid, and manages admissions control.

#### **Etcd**

This is the clusters database, it’s job is to reliably store the state of the cluster. This includes all the cluster configuration and more dynamic information, such as what nodes are part of the cluster, what pods should be running, and where they should be running. You never directly interact with etcd instead, the Kube API server interacts with it on behalf of the cluster.

#### **Kube Scheduler**

It is responsible for scheduling pods onto the nodes, to do that, it evaluates the requirements of each individual pod and selects which node is most suitable but it doesn't do the work of actually launching pod on the node. Instead, whenever it discovers a pod object that doesn't yet have an assignment to a node, it chooses a node and simply writes the name of that node into the pod object. Another component of the system is then responsible for then lunching the pod.

Kube scheduler decides where to lunch a pod because it knows the stores of each node and obeys the constraints that you define, on hardware, software, and also policy. For example, you might specify that certain pods are allowed to run on hardware with a certain amount of memory.

#### **Kube Controller Manager**

The Kube controller has a broader job, it continuously monitors the state of a cluster through the Kube API server. Whenever the state of the cluster doesn't match the state of the desired state, the Kube controller attempts to make changes to achieve the desired state.

Each node runs a small family of the control-plane too. For example, each node runs a Kublet

##### Kublet

u can think of a Kublet as a Kubernetes agent on each node. When the Kube API server wants to start a Kube on a node, it connects to that node’s Kublet. Kublet uses the container runtime to start the pod and monitors its life cycle including readiness and liveness probe and reports back to the Kube API server

##### Kube Proxy

Kube proxy’s job is to maintain network connectivity among the pods in the cluster. In Open source Kubernetes, it does so using the firewall and capabilities of IP tables which is built into the Linux Kernel.

![](https://lh6.googleusercontent.com/aF5bM5vSk2ggqBVSduhs67zhcMx7u_KWDxEkUs4gx3JnUGXzZHYDMG9QSgaOgsfJ9Xln8Ft5pGCZwwDXUscJ0AyvY_at4SVsqoQk_JZLZT9o4PPPZM62wrjqxfCc3foOa9j7UuuB =624x315)