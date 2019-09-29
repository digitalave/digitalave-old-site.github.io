---
layout: post
authors: [dimuthu_daundasekara]
title: 'Lession 01 : Install Docker on CentOS8 /Redhat RHEL8'
image: /images/Docker-Installation/docker.jpg
tags: [Docker, Containers,MicroServices, CentOS 8,RHEL 8]
category: Spring
comments: true
---
### Install Docker on CentOS8 / Redhat RHEL8

##### Introduction

In this very first, tutorial I'm going to demonstrate pre & post installation steps for Docker. In here I'm show you how to install Docker on Windows & CentOS 8 /RHEL 8 step by step.

Before the installation, We should know about little bit about what the Docker is ?

This is the first session of the Docker lesson series. In this tutorial I'm going to teach you about following...

1. What Is Docker ?

2. Docker Editions ?

3. What is Docker Containers ?

4. Docker Installation on CentOS 8

5. Docker Installation on Windows 10

##### What is Docker ?

* Docker is a set of platform-as-a-service.


* That use OS-level virtualization to deliver software in packages.
* They are called containers.


* These containers are isolated from one another and bundle their own software, libraries and configuration files
* They can communicate with each other.


* Docker turns your Computer into isolated containers which runs your codes or services independently. The single operating system carved up into isolated little spaces.


##### Overview of Docker editions:


**Docker is available in three Editions:**

* Docker Engine - Community

* Docker Engine - Enterprise

* Docker Enterprise

<img src="/images/Docker-Installation/1.jpg" width="100%">


**Docker Engine - Community Edition:** 

Good starting point for individual developers, small teams and those who are learning docker.

**Docker Engine - Enterprise Edition:**

Designed for enterprise level development of docker containers with better enhanced security.

**Docker Enterprise:**

Designed for enterprise development and IT teams who build, ship, and run business critical apps in production.

##### What is a Container ?

A self-contained sealed unit of software. It has everything in the container required to run the code.

A Single Container Includes:

1. Code 
2. Configs
3. Processes
4. Networking Services
5. All Dependencies for your code need to run
6. Operating System


<u>Supported Platforms</u>

<img src="/images/Docker-Installation/2.jpg" width="100%">


##### Install Docker Engine - Community For CentOS 8

**OS requirements:**

`centos-extras` repository must be enable. This is enable by default on CentOS.

The `overlay2` storage driver is recommended.

### Install From Docker yum repository


**STEP 01: SETUP THE DOCKER YUM REPOSITORY**

Install required packages For YUM repository management.

```bash
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```


Download and setup docker repository from docker.com

```bash
sudo yum-config-manager  --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

Install docker package

```bash
sudo yum install docker-ce docker-ce-cli containerd.io
```


**STEP 02: Start Docker Service** 

```bash
sudo systemctl start docker
```

**STEP 03: Verify Installation**
Now check whether docker has been installed correctly.

```bash
sudo docker run hello-world
```

which automatically start to download basic "hello-world" images from Docker Hub. 

### Manage Docker as a non-root user:

If you don’t want to preface the docker command with sudo, create a Unix group called docker and add users to it. When the Docker daemon starts, it creates a Unix socket accessible by members of the docker group.

**STEP 01: Create The Docker Group:**

```bash
sudo groupadd docker
```

**STEP 02: Add User To Docker Group:**

```bash
sudo usermod -aG docker $USER
```
In here $USER automatically resolve current user logged in.

Now, You need to logout from $USER and log into $USER again.

$USER is the username which user wants to run without "sudo" prefix.

Verify that you can run docker commands without sudo

```bash
docker run hello-world
```

**STEP 03: Configure Docker to Start on Boot:**

```bash
sudo systemctl enable docker
```

### Install Docker on Ubuntu/Debian 


**Install Docker Engine - Community** 

**A. Setup apt Repository**

```bash
sudo apt-get update
```


**B. Install required Packages to Allow apt Repositories over HTTPS.**

```bash
sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```

 
**C. Add Docker's GPG Key** 
 
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```


**D. Setup Docker Repository**

```bash
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

   
**E. Update apt Package Index Again**

```bash
sudo apt-get update
```


**F. Install Docker Engine**

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io
```


**G. Verify The Docker Engine Installed Correctly**

```bash
sudo docker run hello-world
```


#### Post Installation - Manage Docker as a Non Root User

**A. Create a group  named "docker"**

```bash
sudo groupadd docker
```


**B. Add User to the group named "docker"**

```bash
sudo usermod -aG docker $USER
```


**C. Restart the Host Machine** 

```bash
reboot
```


**D. Verify the Docker Installation** 

```bash
docker run hello-world
```


**E. Docker Start on Boot**

```bash
sudo systemctl enable docker
```

Grate, Now Docker installation on CentOS / Ubuntu has been completed. Now, Lets start to get hands dirty...

Hope this helps for those who are looking for a starting point to learn Docker. I wish to teach more about Docker in the next lesson. 
You can keep in touch  with the future tutorials by  Subscribing me on Youtube.


[<img src="/images/Docker-Installation/sub.gif">] {:target="_blank"} (http://google.com.au/) 

[![Foo](/images/Docker-Installation/sub.gif)](http://google.com.au/)