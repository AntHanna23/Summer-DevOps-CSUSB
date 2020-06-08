# Meeting Two (Setting up Machine, Docker installed First Docker command)
Today we are going to get set up to start learning basic docker fundamentals. Later in the course we will be using GCP but for now we want to not use any extra money so we will be developing locally. Virtual Machines are an easy way to do this, but cloud and some other options will also work. 

## Questions That Will Be Answered

* How to set up virtual machine?
* Installing the latest version of Docker?
* Setting up VsCode and remote development?
* What is Github and creating first Repo for Dockerfile?
* Generating keys for Github
* What are basic Docker commands used to start containers

## Set Up
Where your machine runs is up to you. It needs to be a Linux machine (preferably Ubuntu) and cannot be WSL because docker will not work in that. Docker for Windows is an option but I am not comfortable with it so unless you are already experienced with it I would suggest to stay away from that. 

## Installation of Docker //TODO add how to get the newest version
It is easy to download Docker using the respective repository to the OS. The only issue is that this gives you a slightly older version. We will go over getting the newest version but this will suffice if you happened to have missed class

#### Ubuntu
    sudo apt update && sudo apt upgrade -y && sudo apt install docker.io -y  


#### CentOS
    sudo yum update && sudo yum upgrade -y && sudo yum install docker.io -y
    
## Creating Github //TODO Step by Step on creating keys and pulling repo
Just go to the site and make an account. Make a repository and call it Docker practice. 

    https://www.youtube.com/watch?v=3aKda-oXWc8

Watch this video to set up SSH keys and pull repo to start locally developing Containers

## Docker Commands //TODO add this entire section
