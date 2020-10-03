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

Once the authorized_keys file contains the public key, you need to update permissions on some of the files. The \~/.ssh directory and authorized_keys file must have specific restricted permissions (700 for \~/.ssh and 600 for authorized_keys). If they don't, you won't be able to log in.

Make sure the permissions and ownership of the files are correct.
run the following on both ec2 machines

{< highlight bashrc  >}}
chmod -R go= \~/.ssh
chown -R $USER:$USER \~/.ssh
{{< / highlight >}}

now test if you can connect to the servers. From your local machine or Ansible control node, run:

{< highlight bashrc  >}}
ansible all -m ping -u ubuntu
{{< / highlight >}}
if all goes well, you should see an output that looks like this:

![](/uploads/ping.png)

Once you get a `"pong"` reply back from a host, it means you’re ready to run Ansible commands and playbooks on that server.

### Step 6 - Writing Ansible Playbook

#### Getting Started

Before we can move to a more hands-on view of Ansible, it is important that we get acquainted with important terminology and concepts introduced by this tool.

#### Terminology

The following list contains a quick overview of the most relevant terms used by Ansible:

* **Control Node**: the machine where Ansible is installed, responsible for running the provisioning on the servers you are managing.
* **Inventory**: an `INI` file that contains information about the servers you are managing.
* **Playbook**: a `YAML` file containing a series of procedures that should be automated.
* **Task**: a block that defines a single procedure to be executed, e.g.: install a package.
* **Module**: a module typically abstracts a system task, like dealing with packages or creating and changing files. Ansible has a multitude of built-in modules, but you can also create custom ones.
* **Role**: a set of related playbooks, templates, and other files, organized in a pre-defined way to facilitate reuse and share.
* **Play**: provisioning executed from start to finish is called a _play_.
* **Facts**: global variables containing information about the system, like network interfaces or operating system.
* **Handlers**: used to trigger service status changes, like restarting or reloading a service.

  Now let’s have a look at a playbook that will automate the installation of an Apache webserver within an Ubuntu 18.04 system.

{< highlight yml  >}}
- hosts: all
  become: true
  vars:
    doc_root: /var/www/example
  tasks:
    - name: Update apt
      apt: update_cache=yes

    - name: Install Apache
      apt: name=apache2 state=latest

    - name: Create custom document root
      file: path={{ doc_root }} state=directory owner=www-data group=www-data

    - name: Set up HTML file
      copy: src=index.html dest={{ doc_root }}/index.html owner=www-data group=www-data mode=0644

    - name: Set up Apache virtual host file
      template: src=vhost.tpl dest=/etc/apache2/sites-available/000-default.conf
      notify: restart apache
  handlers:
    - name: restart apache
      service: name=apache2 state=restarted
{{< / highlight >}}

Let’s examine each portion of this playbook in more detail:

hosts: all
The playbook starts by stating that it should be applied to all hosts in your inventory (hosts: all). It is possible to restrict the playbook’s execution to a specific host, or a group of hosts. This option can be overwritten at execution time.

become: true
The become: true portion tells Ansible to use privilege escalation (sudo) for executing all the tasks in this playbook. This option can be overwritten on a task-by-task basis.

vars
Defines a variable, doc_root, which is later used in a task. This section could contain multiple variables.

tasks
The section where the actual tasks are defined. The first task updates the apt cache, and the second task installs the package apache2.

The third task uses the built-in module file to create a directory to serve as our document root. This module can be used to manage files and directories.

The fourth task uses the module copy to copy a local file to the remote server. We’re copying a simple HTML file to be served as our website hosted by Apache.

handlers
Finally, we have the handlers section, where the services are declared. We define the restart apache handler that is notified from the fourth task, where the Apache template is applied.