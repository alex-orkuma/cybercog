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
* You will how to write your ansible-playbook
* you will learn more AWS and cloud computing

## How Does Ansible Work?

Ansible works by configuring client machines, referred to as _managed nodes_, from a computer that has the Ansible components installed and configured, which is then called the _Ansible control node_.

It communicates over normal SSH channels to retrieve information from remote systems, issue commands, and copy files. Because of this, an Ansible system does not require any additional software to be installed on the client computers.

### Step 1- setting up AWS instances

* To begin, log in to your AWS console, navigate to ec2 and select lunch an instance
* On the Choose AMI (Amazon machine image Page), select **Ubuntu Server 18.04 LTS (HVM), SSD Volume Type**
* Next instance type select **t2.micro**
* Next on the configure instance page, change the number of the instance to 2
* select next, on all the next pages.
* click lunch, on the lunch page, select **create a new key-pair**, give the key-pair a name, download key then lunch instance.

#### Connect to one of the Ec2 instances

![](/uploads/screenshot-from-2020-10-03-22-09-03.png)on the ec2 dashboard, right-click on one of the ec2 instances and click connect, follow the instructions to connect to your ec2 machine using ssh. The commands you run should look something like this.![](/uploads/screenshot-from-2020-10-03-22-12-04.png)

### Step 2 — Installing Ansible

To begin using Ansible as a means of managing your server infrastructure, you need to install the Ansible software on the machine that will serve as the Ansible control node.

From your control node, run the following command to include the official project’s PPA (personal package archive) in your system’s list of sources:

{{< highlight bashrc  >}}
sudo apt-add-repository ppa:ansible/ansible
{{< / highlight >}}

Press ENTER when prompted to accept the PPA addition.
Next, refresh your system’s package index so that it is aware of the packages available in the newly included PPA:
{{< highlight bashrc  >}}
sudo apt update
{{< / highlight >}}

Following this update, you can install the Ansible software with:

{{< highlight bashrc  >}}
sudo apt install ansible
{{< / highlight >}}
Your Ansible control node now has all of the software required to administer your hosts. Next, we will go over how to add your hosts to the control node’s inventory file so that it can control them.