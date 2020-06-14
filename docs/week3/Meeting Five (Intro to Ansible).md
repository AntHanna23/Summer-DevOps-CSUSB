# Meeting Five (Intro To Ansible)

## Questions That Will Be Answered
* What is Ansible?
* Why is it needed and why is it becoming so popular now?
* What is an Ansible playbook?
* What is an Ansible role?
* How do we connect to remote machines using Ansible?
* What is an Ansible module?
* Can I make my own modules?

## Google Cloud Platform
We will be using GCP to run Ansible commands and practice configuring remote machine with Ansible. Make sure you have been invited tou you respective GCP project. Below is how to add SSH keys to authenticate Ansible with public keys. I will show you how to do this with SSH certs, but if you missed today the directions below will work just fine. Also make sure that your Ansible account can run sudo commands with a password. 

## What is Ansible:
- Ansible is an open source IT configuration management, deployment, and orchestration tool. It empowers DevOps teams to define their infrastructure as a code in a simple and declarative manner.
- A lot of people compare Ansible to similar tools like Chef or Puppet. They all help automate and provision infrastructure, but there are a few features that make me prefer Ansible over the others.
	- Agentless
	- Controlled by a Management server or Ansible Tower
	- Based in python
	- Fast

## Setting up the Evironment:
- Management server can be a linux distribution of your choice
	- Refrain from using distros like Kali or any other distribution that have a niche purpose
	- Examples of acceptable distros are Ubuntu, CentOs, Mint, Fedora, Manjero. For people that want a challenge, Arch are Gentoo are options, and if you have trouble feel free to ask me for help. I have used them both
	- I will most likely be using a minimal iso but if you feel more comfortable with a GUI feel free to have one, but we will be spending 99.99999999% of our time in the command line 
	- Install Ansible, pip, and your favorite text editor (I will be using vim but nano and others are acceptable)
- Creating your Management server
	- After downloading the distribution of your choice, create a vm. Create an admin user that you will use (do not use root as this is best practice)
	- Make sure your Management server is updated
		- $ sudo apt update && upgrade
		- $ sudo yum upgrade
	- Now install ansible, this may vary because of the operating system you are using. I will be using CentOS because we get a good taste of a Debian based OS when using Kali
- Create two servers that we will be hardening with Ansible
	- One will be CentOS and one will be Ubuntu
	- In CentOs edit the /etc/sysconfig/network-scripts/ifcfg-eth0 to force the server the give the vm an ip address onboot
	- Create an ansible user on both boxes, add ansible to wheel/admin groups and change viduso to no password
- Create ssh key pairs and dns names
	- Add Ip and dns name to the ansible management server in the /etc/hosts file
	- To add make ssh have not password going to server run 
## Setting SSH Key Pair On CentOS & Ubuntu:
- Start off by logging onto you management server and running $ ssh-keygen
- From here put the id_rsa in the .ssh (I usually give it another name but this works well enough for the vm environment. This is not best practice)
- Next run $ ssh- copy-id -i yourfile.pub ansible@destination
- Add 'eval $(ssh-agent)' to your ~/.bash_profile
- Whenever you start your management server run ssh-add (private key), type your pass phrase and you should be able to authenticate without a password while that session exist

[name_of_server_block] <br>
domainname<br>
domainname

[all:vars]<br>
ansible_user=ansible

 ## Moving Ansible Folder and Mapping Inventory:
- Start by taking the /etc/ansible folder and moving it to your home directory with $ mv /etc/ansible ~/ansible
- Remap hosts and roles to look in the current directory by delete the absolute paths in the ansible.cfg file inside the ansible folder
 ## First Ad Hoc Command:
- We will be pinging the servers 
- Make sure that you have loaded the the ssh private key into memory and set up your host file
- In the ansible directory run $ ansible -i hosts all -m ping and make sure you get green back for a successful connection
- the -i specifies the hosts file, all means all possible hosts in that file, and the -m means there will be an ad-hoc command coming a.k.a ping
## Short Playbook Demo:
- Update services and install vim,nginx, and lolcat
- Make change to sshd config
- restart service

```

- hosts: ansibleproject
  become: yes
  tasks:
   - name: Upgrade all packages to the latest version
     package:
      name: "*"
      state: latest

   - name: Install packages
     package:
      name: "{{ packages }}"
     vars:
      packages:
       - vim
       - git
       - nginx 
       - lolcat

   - name: Ensure SSH Protocol is set to 2
      lineinfile:
        state: present
        dest: /etc/ssh/sshd_config
        regexp: '^Protocol'
        line: 'Protocol 2'
     notify: reload sshd

  handlers:
    - name: reload sshd
      service:
        name: ssh
        state: restart

```

---

## Tasks:
- Each play contains a list of tasks. Tasks are executed in order, one at a time, against all machines matched by the host pattern, before moving on to the next task. It is important to understand that, within a play, all hosts are going to get the same task directives. It is the purpose of a play to map a selection of hosts to tasks.
- Tasks are the basic commands of Ansible. These are where you declare things to happens
## Handlers:
- Handlers are assigned to task but are only ran once at the end of a playbook if one of that task register as a change
- They are usually to notify a service to restart 
- handler names much mach in the task part of the playbook
## Templates
- Uses Jinja2 to hold template files and place them where ever you choose
- Jinja is just a text file that is used as a variable and can be placed anywhere in the system with a task
- The thing that makes a template different from the files is that it can be interacted with byt the tasks
- Variables can be put inside these templates and changed based off condition
 ## Files:
- These are straight copies from the server 
- These can not be interacted with because they are not in the jinja2 format 
 ## Vars:
- This is for variables that will be used globally in the role
- defaults page is the same thing but this takes priority over the defaults page
 ## Meta:
- Information about author and the script

## Ansible SetUp
When Ansible starts a runs you will see that it always starts out with gathering facts. Now it is smart to take this out if you don't need it but it can come in handy.

    ansible <host> -m setup

This can be useful in a playbook when doing something like this
```

- name: Gather variables for each operating system
  include_vars: "{{ item }}"
  with_first_found:
    - "{{ ansible_distribution | lower }}-{{ ansible_distribution_version | lower }}.yml"
    - "{{ ansible_distribution | lower }}-{{ ansiyml
    - always

```

## Exercise
* Use Ansible to update everything on the system
* Use Ansible to install docker, sql, postfix, cowsay, and python3
* Change sshd config to not alow Root Login and  use Protocol 2
* Use Ansible handler to restart SSH
* Use ansible set up variable to print the ansible_hostname
* Use Ansible to set SELinux to enforcing
Extra credit: disable xinetd if it exist (use ansible facts)