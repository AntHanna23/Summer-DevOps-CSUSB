# Meeting Three(What is Docker and Fundamentals)
Today is going to get us into the command line and running some Dcoker commands and learning some of the fundamental things that are needed to run docker effectively. We will also go through and do some excersizes to put som eof the things that we learn into practice. 

## Questions That Will Be Answered
* What is the difference between a container and a VM?
* Is Docker Virtualization?
* How does Docker networking work?
* Can I get the command line of my container?
* Can I run multiple containers at the same time?
* Can Docker loadbalance on the same docker network


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
First we will start a container with nginx web server. We want to be able to connect to the conatiner from the outside world. We will pull an Nginx Image, run it, see the process and then delete it. 

    docker container run -p 80:80 nginx

This run the container and forwards the first port (80) to the Docker port(also 80). Now if you go to your web browser and type localhost it will show the Nginx Default screen. Run control-C to to stop the process an get your terminal back. Now lets elaborate a little more on that docker run command and make it a little more realistic to something you would actually run.

    docker container run --name web -d -p 1234:80 --rm  nginx 

This docker command uses the -d which detaches the conatiner from the terminal, -p works the same as before but now you would connect on localhost:1234. The --rm removes the container whenever it is stopped.

    docker ps

    docker container ls

    docker ps -a

Now we can see the new conatiner running. We can also see the old one because though it is stopped it has not been removed and therefore can be restarted. Remove the old container run: 

    docker stop $CONTAINER_HASH

Now with the running container we will log into the command line to change an issue on the Nginx Default page.

    docker exec -it $CONTAINER_NAME /bin/bash

Now change the default file in /var/www/default and restart the Nginx service to see the change on the website.

Let's see that the 

Now stop all running containers by running 

    docker stop $(docker ps -aq)

Now for an excercise for the participants to try out. 

 



