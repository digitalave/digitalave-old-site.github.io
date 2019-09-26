---
layout: post
authors: [dimuthu_daundasekara]
title: 'Lession 01 : Get Started With Docker on Windows 10 & CentOS7/RHEL8'
image: /images/Docker-Installation/docker.jpg
tags: [Docker, Containers,MicroServices, CentOS 8,RHEL 8]
category: Spring
comments: true
---



### Overview of Docker editions:
In this very begin tutorial I'm going to demonstrate pre & post installation steps for Docker. In here I'm show you how to install Docker on Windows & CentOS 8 /RHEL 8 step by step.

**Content:** 

* Install Docker Desktop For Windows

* Install Docker Engine Community on CentOS 8 / RHEL 8

* Manage Docker as a non-root user

**Docker is available in three tiers:**

* Docker Engine - Community

* Docker Engine - Enterprise

* Docker Enterprise

<img src="/images/Docker-Installation/1.jpg" width="100%">


**Docker Engine - Community Edition:** 

Ideal for individual developers and small teams. Good  point to get stared with Docker container-based apps.

**Docker Engine - Enterprise Edition:**

Designed for enterprise development of containers with enhanced security and meet with enterprise grade Service Level Agreements

**Docker Enterprise:**

Designed for enterprise development and IT teams who build, ship, and run business critical apps in production.

#### About Docker Engine - Community

Docker Engine - Community is ideal for developers and small teams looking to get started with Docker and experimenting with container-based apps.

<u>Supported Platforms</u>

<img src="/images/Docker-Installation/2.jpg" width="100%">


#### Install Docker Desktop on Windows 

* Also know as Docker Desktop

* Based on Community Edition of Docker

* Runs of Windows

* Easy to use development environment for building, shipping, running containers and pushing into production.


Look for system requirements before you begin the installation.

#### System Requirements:

* Windows 10

* 64Bit Processor 

* 4GB RAM

* BIOS-Level Hardware Virtualization Support Must Be Enabled in the BIOS Settings.

**What's Included in  the Docker Desktop Installer ???**

* Docker Engine

* Docker CLI Client

* Docker Compose

* Docker Machine

* Kitematic - Docker Graphical Tool which  allows you to download from the Docker Hub.

You can download Docker Desktop for  Windows from  Docker Hub.

<a href="https://hub.docker.com/?overlay=onboarding" target="_blank">Download Docker Desktop For Windows</a>

#### Install Docker Desktop on Windows:

Download Docker Desktop For Windows and run the installer.

Accept License. 

Start Docker Desktop from start menu.

<img src="/images/Docker-Installation/3.jpg" width="100%">


<img src="/images/Docker-Installation/4.jpg" width="100%">


<img src="/images/Docker-Installation/5.jpg" width="100%">

#### Install Docker Engine - Community For CentOS 8


**OS requirements:**


`centos-extras` repository must be enable. This is enable by default on centos.
The `overlay2` storage driver is recommended.


##### Install From Docker yum repository

**STEP 01: SETUP THE DOCKER YUM REPOSITORY**

```bash
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```


```bash
sudo yum-config-manager  --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```


```bash
sudo yum install docker-ce docker-ce-cli containerd.io
```


**STEP 02: Start Docker Service** 

```bash
sudo systemctl start docker
```

**STEP 03: Verify Installation**

Verify Docker Engine - Community is installed correctly.

```bash
sudo docker run hello-world
```

> 2. Install from a package
> 
> https://download.docker.com/linux/centos/7/x86_64/stable/Packages/
> 
> sudo yum install /path/to/package.rpm
> 

#### Manage Docker as a non-root user:

If you don’t want to preface the docker command with sudo, create a Unix group called docker and add users to it. When the Docker daemon starts, it creates a Unix socket accessible by members of the docker group.

**STEP 01: Create The Docker Group:**


```bash
sudo groupadd docker
```

**STEP 02: Add User To Docker Group:**

```bash
sudo usermod -aG docker $USER
```

$USER is the username which user wants to run without "sudo" prefix.

Verify that you can run docker commands without sudo

```bash
docker run hello-world
```


STEP 03: Configure Docker to Start on Boot:

```bash
sudo systemctl enable docker
```





