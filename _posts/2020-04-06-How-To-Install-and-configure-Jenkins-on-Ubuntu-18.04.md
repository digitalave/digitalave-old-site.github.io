---
layout: post
authors: [dimuthu_daundasekara]
title: 'How to Install & Configure Jenkins on Ubuntu 18.04 LTS / 16.04 / Debian'
image: \images\jenkins-ubuntu\Jenkins.jpg
tags: [Jenkins, CICD, Automation,Continuous Integration, Continuous Delivery]
category: Spring
comments: true
---
<img src="\images\jenkins-ubuntu\Jenkins.jpg" width="100%">
## How to Install & Configure Jenkins on Ubuntu / Debian (16.04LTS,18.04LTS)

### Introduction:

Jenkins is an open source automation server which provides hundreds of plugin to perform continues integration and continues delivery for building and deploying projects.

Continuous integration (CI) is a DevOps practice in which team members regularly commit their code changes to the version control repository, after which automated builds and tests are run. Continuous delivery (CD) is a series of practices where code changes are automatically built, tested and deployed to production.

If you are a software developer, Then surely Jenkins suites for you and automate your CI/CD build tasks easily.

Jenkins can automate continuous integration and continuous delivery (CI/CD) for any project.
Support for hundreds of plugins in the update center.  Which provides infinite possibilities for what Jenkins can do.
Jenkins can configure into distributed system that distribute work across multiple node/machines.

### Features: 

**CICD** - Continues Integration and Continues Delivery

**Plugins** - Hundreds of Plugin Support

**Extensible** - Extended possibilities using its Plugins

**Distributed** - Distribute work across multiple machines and projects

### Prerequisites:

`Hardware - RAM > 1GB | HDD > 50GB+`
`Software - JAVA (JRE 8 | JDK 11)`

Following JDK/JRE Versions support for current Jenkins versions

`OpenJDK JDK / JRE 8 - 64 bits`
`OpenJDK JDK / JRE 11 - 64 bits`

**NOTE: Always check JAVA version requirement before proceed the Jenkins installation.**

REF: <a href="https://www.jenkins.io/doc/administration/requirements/java/" target="_blank">https://www.jenkins.io/doc/administration/requirements/java/</a>


### STEP 01: Install Oracel/Open JDK 11

##### Install OpenJDK

```bash
sudo apt update

sudo apt-get install openjdk-11-jdk -y
```
OR

REF: <a href="https://www.oracle.com/java/technologies/javase-jdk11-downloads.html" target="_blank">https://www.oracle.com/java/technologies/javase-jdk11-downloads.html</a>

```bash
sudo dpkg -i jdk-11.0.7_linux-x64_bin.deb
```

##### Set Default JDK Version

```bash
sudo update-alternatives --config java

java -version
```

### STEP 02: Install Jenkins Using Debian Repository

##### Import trusted PGP Key for Jenkins

```bash
sudo wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
```

##### Add Jenkins Repository

sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'

##### Install Jenkins

```bash
sudo apt-get update

sudo apt-get install jenkins
```

Jenkins service will automatically start after the installation process is complete. You can verify it by printing the service status

```bash
sudo systemctl status jenkins
```

If it isn't enabled/started automatically.

```bash
sudo systemctl enable jenkins

sudo systemctl start jenkins
```

### STEP 03: Configure Firewall

Allow port 8080 through the Ubuntu firewall

```bash
sudo ufw allow 8080/tcp
```

### STEP 04: Initial Setup For Jenkins

Once above configuration completed, Open-up your web browser and access through the IP:PORT.

> http://your_ip_or_domain:8080 

Now, Head-over to terminal again, and find out the Administrator password using  this command

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

<img src="\images\jenkins-ubuntu\1.png" width="100%">

Copy the password from terminal and paste it in the required field



On the next screen at the initial setup wizard will ask for install suggested plugins or you want to select specific plugins. Click on the Install suggested plugins box, and the installation process will start immediately.

<img src="\images\jenkins-ubuntu\2.png" width="100%">

<img src="\images\jenkins-ubuntu\3.png" width="100%">

Once the plugins are installed, you will be prompted to set up the first admin user. Fill out all required information and click Save and Continue.

<img src="\images\jenkins-ubuntu\4.png" width="100%">

<img src="\images\jenkins-ubuntu\5.png" width="100%">

Click on the Start using Jenkins button and you will be redirected to the Jenkins dashboard logged in as the admin user you have created in one of the previous steps.

<img src="\images\jenkins-ubuntu\6.png" width="100%">

<img src="\images\jenkins-ubuntu\7.png" width="100%">

Now you've successfully installed Jenkins on your Ubuntu system.

##### Bottom Line

##### In this tutorial, you have learned how to install and perform the initial configuration of Jenkins. 

##### If you have any questions, Please leave a comment on the YouTube comment section. 









