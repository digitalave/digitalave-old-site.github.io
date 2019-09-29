---
layout: post
authors: [dimuthu_daundasekara]
title: 'Lession 02 : Docker Basics - Where Am I Start Learn Docker ?'
image: /images/Docker-Installation/docker.jpg
tags: [Docker, Containers,MicroServices, CentOS 8,RHEL 8]
category: Spring
comments: true
---

In this tutorial I'm going to teach you the basics you should have known before dive into depth.

Now, I'm going to cover up fundamentals you must know.


Understand Docker Images:

A. List Docker Images

$docker images

Repository - Image where it came from 
TAG - Version Number
Image ID - Internal representation of Docker Image

B. Running an Image

$docker run -ti ubuntu:latest bash 

Docker image tuns into a running container. Docker run takes images into container.

-ti  Terminal Interactive - stands for interactive shell (Which help  for  TAB Completion & work correctly)

bash - Run command on a bash shell

Now I'm in a Ubuntu  environment

Now check 

#cat /etc/lsb-release 

To exit

#exit


D. List the Running Images

$docker ps

ID : Running Container's ID
IMAGE : Image which made from
COMMAND : Command it running on
STATUS  
PORT


E. List the Recently Stopped Last Containers 

$docker ps -l

F. List All Stopped Containers

$docker ps -al

G. Docker Commit 
Docker Commit takes containers back to new images.

$docker run -ti ubuntu:latest bash 

#touch test.file

#exit

$docker ps -l

ID : XXXXXXXX
IMAGE : ubuntu
COMMAND  : bash 
CREATED :
STATUS : 
PORTS :
NAMES : ubuntu_happy

$docker commit <Container ID>

OR

$docker commit <NAMES>

Which returns Image ID
sha256:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

Now, I have a newly created Docker Images.

H. Give Images a Name Using TAG

$docker tag <Image ID> my-new-image

Now List Docker Images

$docker images

I. Now, Run Docker Image using The TAG

$docker run -ti my-new-image bash

#ls -la /test.file

Now, It has my  test.file in it.

###################################

How To Run Processes In Docker Containers

$docker run --rm -ti ubuntu sleep 5

$docker run -ti ubuntu bash -c "sleep 3; echo  all done"

--rm Delete this container when it exits 

-ti Interactive Terminal

Keep Things Running  on Container

$docker run -d -ti ubuntu

-d detach things running and leaving  running in the background 

Now, Jump into  the task again that I had started over there.

$docker attach <container_name>

By Pressing  Ctrl+P, Ctrl+Q these key combinations you can exit from  terminal by detaching from it. But, leaves it running. So you can attach back again. 

again you can find running  docker container using "docker ps" and you can run it again using "docker attach <container name>"

Docker Logs:
By looking at the container output. Even though, you start the container, but it didn't work. 
Now "docker logs" command come to  an action.

$docker run --name example -d ubuntu bash -c "lose /etc/passwd"
docker logs <container_name> use look at what the output was.

docker logs <container_name>

$docker logs exmaple

Stopping  & Removing Containers:


docker kill <container_name> Its stop the container.

docker rm <container_name> Its remove the container.

$docker kill <container_name>
docker rm <container_name>
$docker ps -l


Docker Resource Limit Enforce Management:
Docker has a feature to limit & enforce limit the resources. 
You can limit it to  a fixed amount of memory as you need.



