---
title: Using Ansible to Manage Two AWS Ubuntu Servers
subtitle: Getting started with Ansible
date: 2020-10-02T23:00:00+00:00
tags:
- playbooks
- ssh
- cloud computing
- aws
- ubuntu
- Ansible
draft: true

---
## Business Problem

You are an IT administrator for company **XYZ,** you configure, manage, update, and provision servers on weekly basis. For the most part, you do it manually and use a bash script to automate a few things. The head of IT asks you to look for a way to automate this whole process.

## Solution 

Ansible is a configuration management tool that is designed to automate controlling servers for administrators and operations teams. With Ansible you can use a single central server to control and configure many different remote systems using SSH and Python as the only requirements.

In this guide, you will learn

* You will learn how to use Ansible to control, configure and install docker on many ubuntu servers hosted on AWS
* You will how to write your own ansible-playbook
* you will learn more AWS and cloud computing

## How Does Ansible Work?

Ansible works by configuring client machines, referred to as _managed nodes_, from a computer that has the Ansible components installed and configured, which is then called the _Ansible control node_.

It communicates over normal SSH channels to retrieve information from remote systems, issue commands, and copy files. Because of this, an Ansible system does not require any additional software to be installed on the client computers.

### Step 1- setting up AWS instances

* To begin, log in to your AWS console, navigate to ec2 and select lunch an instance
* On the Choose AMI (Amazon machine image Page), select **Ubuntu Server 18.04 LTS (HVM), SSD Volume Type**