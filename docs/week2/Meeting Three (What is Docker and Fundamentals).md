# Meeting Three(What is Docker and Fundamentals)
Today is going to get us into the command line and running some Docker commands and learning some of the fundamental things that are needed to run docker effectively. We will also go through and do some excersizes to put som eof the things that we learn into practice. 

## Questions That Will Be Answered
* What is the difference between a container and a VM?
* Is Docker Virtualization?
* How does Docker networking work?
* Can I get the command line of my container?
* Can I run multiple containers at the same time?
* Can Docker load balance on the same docker network


## Docker Commands 

Here are some Docker commands that we are going to quickly introduce so that we may continue onto Docker files next week.

    docker login - prompts you to long into docker hub so that you can push and pull images from your account

    docker start – Starts one or more stopped containers

    docker stop – Stops one or more running containers

    docker build – Builds an image form a Docker file

    docker pull – Pulls an image or a repository from a registry

    docker push – Pushes an image or a repository to a registry

    docker exec – Runs a command in a run-time container

## Starting an Nginx Container
First we will start a container with nginx web server. We want to be able to connect to the container from the outside world. We will pull an Nginx Image, run it, see the process and then delete it. 

    docker container run -p 80:80 nginx

This run the container and forwards the first port (80) to the Docker port(also 80). Now if you go to your web browser and type localhost it will show the Nginx Default screen. Run control-C to to stop the process an get your terminal back. Now lets elaborate a little more on that docker run command and make it a little more realistic to something you would actually run.

    docker container run --name web -d -p 1234:80 --rm  nginx 

This docker command uses the -d which detaches the container from the terminal, -p works the same as before but now you would connect on localhost:1234. The --rm removes the container whenever it is stopped.

    docker ps

    docker container ls

    docker ps -a

Now we can see the new container running. We can also see the old one because though it is stopped it has not been removed and therefore can be restarted. Remove the old container run: 

    docker stop $CONTAINER_HASH

Now with the running container we will log into the command line to change an issue on the Nginx Default page.

    docker exec -it $CONTAINER_NAME /bin/bash

Now change the default file in /var/www/default and restart the Nginx service to see the change on the website.

Let's see that the 

Now stop all running containers by running 

    docker stop $(docker ps -aq)

## Excecise One
Now for an exercise for the participants to try out. 

* Start an Nginx, Apache, and SQL container ( hint when you start the sql container use -e MYSQL_RANDOM_ROOT_PASSWORD=yes)
* Name them web, httpd, and db respectively
* After they have all show they are up change the default nginx page 
* Get the sql root password
* stop all the containers with a single command and remove the stopped containers

## Docker Networks
Docker has quite a few networking options by default:

* bridge: The default network driver. If you don’t specify a driver, this is the type of network you are creating. Bridge networks are usually used when your applications run in standalone containers that need to communicate
* host: For standalone containers, remove network isolation between the container and the Docker host, and use the host’s networking directly. 
* overlay: Overlay networks connect multiple Docker daemons together and enable swarm services to communicate with each other. You can also use overlay networks to facilitate communication between a swarm service and a standalone container, or between two standalone containers on different Docker daemons. This strategy removes the need to do OS-level routing between these containers.
* macvlan: Macvlan networks allow you to assign a MAC address to a container, making it appear as a physical device on your network. The Docker daemon routes traffic to containers by their MAC addresses. Using the macvlan driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network, rather than routed through the Docker host’s network stack
* none: For this container, disable all networking. Usually used in conjunction with a custom network driver. none is not available for swarm services


These can be seen by running: 

    docker network ls

We can create our own networks and control how our containers talk to each other. Docker also has built in DNS that can load balance across containers with the same network alias. Lets start by creating a network and putting a container inside of the network. 

    docker network create test

    docker container run -d  --network test nginx 

Now we can inspect the pod and see that it is running in the test network. Now give lets stop that container and remove it. Now let us create 2 new containers with aliases.

    docker network create test

    docker container run -d --net test --net-alias web nginx (run this command twice)

    docker container run --net test --rm alpine nslookup web

The last command should return two address and if you use another machine to curl you can see that it switches which it targets every time you run a command.

## Exercise Two

Now for the second exercise:

* Make a network called logging
* Make 3 containers with the elasticsearch:2 image
* Use a distro image (alpine, ubuntu, centos) to do an nslookup and curl the machines


Next we will learn about Docker Images /w Dockerhub and talk about Dockerfiles
 



