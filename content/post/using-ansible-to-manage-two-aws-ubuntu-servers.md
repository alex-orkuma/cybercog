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

![/uploads/screenshot-from-2020-10-03-22-09-03.png](https://app.forestry.io/sites/rmreowx0yfjbvg/body-media//uploads/screenshot-from-2020-10-03-22-09-03.png)

on the ec2 dashboard, right-click on one of the ec2 instances and click connect, follow the instructions to connect to your ec2 machine using ssh. The commands you run should look something like this.![/uploads/screenshot-from-2020-10-03-22-12-04.png](https://app.forestry.io/sites/rmreowx0yfjbvg/body-media//uploads/screenshot-from-2020-10-03-22-12-04.png)

### Step 2 — Installing Ansible

To begin using Ansible as a means of managing your server infrastructure, you need to install the Ansible software on the machine that will serve as the Ansible control node.

From your control node, run the following command to include the official project’s PPA (personal package archive) in your system’s list of sources:

{{< highlight bashrc  >}}
sudo apt-add-repository ppa:ansible/ansible
{{< / highlight >}}

Press ENTER when prompted to accept the PPA addition. Next, refresh your system’s package index so that it is aware of the packages available in the newly included PPA:

{{< highlight bashrc  >}}
sudo apt update
{{< / highlight >}}

Following this update, you can install the Ansible software with:

{{< highlight bashrc  >}}
sudo apt install ansible
{{< / highlight >}}

Your Ansible control node now has all of the software required to administer your hosts. Next, we will go over how to add your hosts to the control node’s inventory file so that it can control them.

### Step 3 — Setting Up the Inventory File

The inventory file contains information about the hosts you’ll manage with Ansible. You can include anywhere from one to several hundred servers in your inventory file, and hosts can be organized into groups and subgroups. The inventory file is also often used to set variables that will be valid only for specific hosts or groups, to be used within playbooks and templates. Some variables can also affect the way a playbook is run, like the ansible_python_interpreter variable that we’ll see in a moment.

To edit the contents of your default Ansible inventory, open the /etc/ansible/hosts file using your text editor of choice, on your Ansible Control Node:

{< highlight bashrc  >}}
sudo nano /etc/ansible/hosts  
{{< / highlight >}}

then edit the file to look like this

![/uploads/ansible.png](https://app.forestry.io/sites/rmreowx0yfjbvg/body-media//uploads/ansible.png)

by adding the following lines of code to it.

{< highlight bashrc  >}}
"under the **Webserver** section, remove the # and  add this" server1 ansible_host=ubuntu@54.212.128.245 server2 ansible_host=ubuntu@52.12.133.253

"add a new section **all:vars** and add the following underneath" ansible_python_interpreter=/usr/bin/python3
{{< / highlight >}}

the **ubuntu@xx.xxx.xxx.xxx** represents your Ubuntu virtual machines. ubuntu is the username and the IP addresses the public IP address of your VM. By default, all ubuntu VMs are created with an ubuntu user with Sudo privileges.

The **all: vars** subgroup sets the _ansible_python_interpreter_ host parameter that will be valid for all hosts included in this inventory. This parameter makes sure the remote server uses the /usr/bin/python3 Python 3 executable instead of /usr/bin/python (Python 2.7), which is not present on recent Ubuntu versions.

When you’re finished, save and close the file by pressing CTRL+X then Y and ENTER to confirm your changes.

Whenever you want to check your inventory, you can run:

{< highlight bashrc  >}}
ansible-inventory --list -y
{{< / highlight >}}

the output should look something like this.

![/uploads/ansi.png](https://app.forestry.io/sites/rmreowx0yfjbvg/body-media//uploads/ansi.png)

### Step 4 — Create SSH Keys with OpenSSH

The standard OpenSSH suite of tools contains the utility, which is used to generate key pairs. Run it on your local computer to generate a 2048-bit RSA key pair, which is fine for most uses.

{{< highlight bashrc  >}}
ssh-keygen
{{< / highlight >}}
The utility prompts you to select a location for the keys. By default, the keys are stored in the \~/.ssh directory with the filenames id_rsa for the private key and id_rsa.pub for the public key. Using the default locations allows your SSH client to automatically find your SSH keys when authenticating, so we recommend accepting them by pressing ENTER.

### Step 5 - Upload the SSH Public Key to the Ubuntu servers to be controlled.

Generally speaking there are many ways this can be done, however some demand certain conditions are met.
we could use **ssh-copy-id** but this requires password based access to our VM. ec2 machines by default is configured for passwordless ssh access. if we type

{{< highlight bashrc  >}}
ssh-copy-id ubuntu@52.12.133.253
{{< / highlight >}}
we should get a **Permission denied (publickey)** error like so

![](/uploads/permission.png)

the best way to do this is to add the key manually to the VMs
In you terminal type

{{< highlight bashrc  >}}
cat \~/.ssh/id_rsa.pub
{{< / highlight >}}
Copy the output.
ssh into the Ubuntu VMs using your local terminal as highlighted above.

On one of the VMs, type
{< highlight bashrc  >}}
nano \~/.ssh/authorized_keys
{{< / highlight >}}

Paste the contents of your SSH key output you copied into the file by right-clicking in your terminal and choosing Paste or by using a keyboard shortcut like CTRL+SHIFT+V. Then, save and close the file. In nano, save by pressing CTRL+O and then ENTER, and exit by pressing CTRL+X.
you should have something that looks like this

![](/uploads/ss.png)

repeat the same procedure for the other VM, and add the public key to authorized keys.

Once the authorized_keys file contains the public key, you need to update permissions on some of the files. The ~/.ssh directory and authorized_keys file must have specific restricted permissions (700 for ~/.ssh and 600 for authorized_keys). If they don't, you won't be able to log in.

Make sure the permissions and ownership of the files are correct.
run the following on both ec2 machines
{< highlight bashrc  >}}
chmod -R go= ~/.ssh
chown -R $USER:$USER ~/.ssh
{{< / highlight >}}