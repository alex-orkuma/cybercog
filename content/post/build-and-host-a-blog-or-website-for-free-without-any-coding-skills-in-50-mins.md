---
title: Build and Host a Blog or Website for Free Without any Coding Skills in 50 mins
subtitle: Learn how build a hugo website or blog for free.
date: 2020-09-13T23:00:00+00:00
tags:
- beginner
- blog
- Hugo
draft: true

---
**Before We Start**

here are a couple of things to keep in mind. 

this is going to be a static website built using [Hugo](https://gohugo.io/documentation/). for a difference between static and dynamic websites read this [article](https://wpamelia.com/static-vs-dynamic-website/#:\~:text=Static%20websites%20are%20ones%20that,databases%20in%20addition%20to%20HTML.).

For this tutorial to work, you need to install Hugo and Golang if you don't already have it installed. 

**Installing Golang**

This tutorial was done using Ubuntu 20.04, the steps to install Golang on your OS might differ. Please refer to the [documentation](https://golang.org/doc/install?download=go1.15.2.linux-amd64.tar.gz#install) for your own use case

* download Golang here [Go](https://golang.org/dl/go1.15.2.linux-amd64.tar.gz "Go")
* extract it into /usr/local, creating a Go tree in /usr/local/go

      tar -C /usr/local -xzf go1.15.2.linux-amd64.tar.gz
* Add /usr/local/go/bin to the `PATH` environment variable

      export PATH=$PATH:/usr/local/go/bin