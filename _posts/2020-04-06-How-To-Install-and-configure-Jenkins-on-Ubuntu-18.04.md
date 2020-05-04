---
layout: post
authors: [dimuthu_daundasekara]
title: 'How to Install & Configure Jenkins on Ubuntu 18.04 LTS / 16.04 / Debian'
image: /images/SonarQube_Ubuntu/1.png
tags: [Jenkins, CICD, Ubuntu]
category: Spring
comments: true
---


## How to Install & Configure Jenkins on Ubuntu 18.04 LTS / 16.04 / Debian

### Introduction:

Jenkins is an open source automation server which provides hundreds of plugins to perform continues integration and continues delivery for building and deploying projects

Continuous integration (CI) is a DevOps practice in which team members regularly commit their code changes to the version control repository, after which automated builds and tests are run. Continuous delivery (CD) is a series of practices where code changes are automatically built, tested and deployed to production.

### Features: 

CICD - Continues Integration and Continues Delivery 
Plugins - Hundreds of Plugin Support
Extensible - Extended possibilities using its Plugins
Distributed - Distribute work across multiple machines and projects

### Prerequisites:

Hardware - RAM > 512MB | HDD > 10GB
Software - JAVA (JRE 8 | JDK 11)

### STEP 01: Install Oracle/Open JDK 11

##### Install OpenJDK

sudo apt update
sudo apt-get install openjdk-11-jdk -y

OR

REF: https://www.oracle.com/java/technologies/javase-jdk11-downloads.html

sudo dpkg -i jdk-11.0.7_linux-x64_bin.deb

##### Set Default JDK Version

sudo update-alternatives --config java

java -version

### STEP 02: Install Jenkins Using Debian Repository

##### Import trusted PGP Key for Jenkins

sudo wget -q -O - http://pkg.jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -

##### Add Jenkins Repository

sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'


##### Install Jenkins

sudo apt-get update 

Jenkins service will automatically start after the installation process is complete. You can verify it by printing the service status

sudo systemctl status jenkins


### STEP 03: Configure Firewall

Allow port 8080 through the ubuntu firewall

sudo ufw allow 8080/tcp

### STEP 04: Initial Setup For Jenkins

Once above configuration completed, Open-up your web browser and access through the IP:PORT.

http://your_ip_or_domain:8080 

IMG1

<img src="/images/SonarQube_Ubuntu/1.png" width="100%">

Now, Head-over to terminal again, and find out the Administrator password using  this command

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Please copy the password from terminal and paste it below.

On the next screen at the initial setup wizard will ask for install suggested plugins or you want to select specific plugins. Click on the Install suggested plugins box, and the installation process will start immediately.

IMG2

IMG3

Once the plugins are installed, you will be prompted to set up the first admin user. Fill out all required information and click Save and Continue.

IMG4

IMG5
Click on the Start using Jenkins button and you will be redirected to the Jenkins dashboard logged in as the admin user you have created in one of the previous steps.

IMG6

IMG7

Now you've successfully installed Jenkins on your Ubuntu system.

#### Conclusion

In this tutorial, you have learned how to install and perform the initial configuration of Jenkins. 

**If you have any questions, please leave a comment on the Youtube comment section.** 









