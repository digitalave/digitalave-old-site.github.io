---
layout: post
authors: [dimuthu_daundasekara]
title: 'How to Install & Configure SonarQube 8 on Ubuntu 18.04 LTS / 16.04 / Debian'
image: \images\jenkins-ubuntu\Jenkins.jpg
tags: [Jenkins, CICD, Automation,Continuous Integration, Continuous Delivery,SonarQube]
category: Spring
comments: true
---

## How to Install and Configure SonarQube 8 on Ubuntu 18.04 LTS / Debian

### Introduction:

SonarQube is an open-source platform. Which uses for static code analysis and continuous inspection of code quality. SonarQube can detect bugs, code smells and security vulnerabilities.SonarQube empowers developers to write cleaner and safer code.

SonarQube is bundled with static code analyzer for more than 27 programming languages.
SonarQube performs continues code inspection using thousands of automated static code analysis rules.

SonarQube allows developers to build bugs free, secure application in many languages. And also maintain a code standards for best developer practices.

SonarQube provides code reliability by preventing bugs and application security by fixing vulnerabilities that compromise your code.
SonarQube is able to integrate with CI/CD tools such as Jenkins, Azure DevOps, GitHub, GitLab, Bitbucket and many more.

### Features:

**Build Integration - Jenkins, Azure DevOps, Bamboo, etc...**

**IDE Integration - Visual Studio, Eclips, InteliJ, etc...**

**Other Pipeline Integration** 

### Prerequisites:

`OS - Ubuntu 18.04 / 16.04 LTS / Debian`

`RAM - 4GB Minimum RAM`

`CPU - 1vCPU`

`JAVA - Oracle JRE 11 or OpenJDK 11`

**NOTE:** *Please make sure to install compatible Java version before continue the installation.*

REF: <a href="https://docs.sonarqube.org/latest/requirements/requirements/" target="_blank">https://docs.sonarqube.org/latest/requirements/requirements/</a>

In this tutorial, I will going to install SonarQube 8.3 Community Edition on Ubuntu 18.04. Which required OpenJDK 11 packages to be installed on the system.

SonarQube 8.3
OpenJDK 11
PostgreSQL 12

### STEP 01: Set kernal Parameters & System Limits

Append these entries to bottom of the "sysctl.conf" file.

```bash
sudo vim /etc/sysctl.conf
```


```bash
vm.max_map_count=262144
fs.file-max=65536
ulimit -n 65536
ulimit -u 4096
```


And, also append these entries at the end of the "limits.conf" file.

```bash
sudo vim /etc/security/limits.conf
```


```bash
sonarqube   -   nofile   65536
sonarqube   -   nproc    4096
```


Make sure to reboot systems once above changes made. Therefore New changes will reflect after the reboot.

### STEP 02: Install OpenJDK 11

**Download & Install JDK 11 APT Repositories**

```bash
sudo apt-get update -y
```


```bash
sudo apt-get install openjdk-11-jdk -y
```


OR

**Download & Install JDK 11 using .deb Package**

REF: <a href="https://www.oracle.com/java/technologies/javase-jdk11-downloads.html" target="_blank">https://www.oracle.com/java/technologies/javase-jdk11-downloads.html</a>

```bash
sudo dpkg -i jdk-11.0.7_linux-x64_bin.deb
```

**Set Default JDK Version**

```bash
sudo update-alternatives --config java
```


**Verify Install Java Version**

```bash
java -version
```


### STEP 02: Install & Configure PostgreSQL Database for SonarQube

In this tutorial I'm going to use PostgreSQL as my database engine
Always better if you check version compatibility matrix,  before install packages. 

REF: <a href="https://docs.sonarqube.org/latest/requirements/requirements/
" target="_blank">https://docs.sonarqube.org/latest/requirements/requirements/
</a>

```bash
sudo apt update
```


**Import Trusted PGP Key and PostgreSQL APT Repo**

```bash
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'

wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -
```


**Install PostgreSQL** 

```bash
sudo apt install postgresql postgresql-contrib
```


**Check PostgreSQL Version**

```bash
sudo -u postgres psql -c "SELECT version();"
```


**Enable  & Start PostgreSQL Service** 

```bash
sudo systemctl enable postgresql.service

sudo systemctl start  postgresql.service
```


**Change PostgreSQL default user password**

```bash
sudo passwd postgres
```


**Switch  to PostgreSQL User**

```bash
su - postgres
```


**Create New User "sonar"**

```bash
createuser sonar
```


**Log Into PostgreSQL Shell**

```bash
psql
```


**Set Password for SonarQube Database User "sonar"**

```bash
ALTER USER sonar WITH ENCRYPTED PASSWORD 'p@ssw0rd';
```


**Create New Database "sonarqube"**

```bash
CREATE DATABASE sonarqube OWNER sonar;
```


**Grant Privileges to "sonar" User on "sonarqube" Database**

```bash
GRANT ALL PRIVILEGES ON DATABASE sonarqube to sonar;
```


Exit From PostgreSQL Shell

```bash
\q
```


**Exit From  "postgres" User**

```bash
exit
```


**Restart & Check PostgreSQL DB Service Status again**

```bash
systemctl restart  postgresql
systemctl status -l   postgresql
```


### STEP 03: Download & Install SonarQube 

**Download SonarQube Archive File**

REF: <a href="https://binaries.sonarsource.com/Distribution/sonarqube/" target="_blank">https://binaries.sonarsource.com/Distribution/sonarqube/</a>

Now, Let's create temporary directory and download SonarQube archive file.

```bash
sudo mkdir /sonarqube/

cd /sonarqube/
```


```bash
sudo curl -O https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-8.3.0.34182.zip
```


Install If "zip" package not available on the system.

```bash
sudo apt-get install zip
```


```bash
sudo unzip sonarqube-8.3.0.34182.zip -d /opt/
```


Move Extracted setup into /opt/sonarqube/ derectory

```bash
sudo mv /opt/sonarqube-8.3.0.34182/ /opt/sonarqube
```


### STEP 04: Create Group & User for SonarQube

**Create a group named "sonar"**

```bash
sudo groupadd sonar
```


**Create a user named "sonaer" and into "sonar" group with directory access**

```bash
sudo useradd -c "SonarQube - User" -d /opt/sonarqube/ -g sonar sonar
```


Provide user & group directory permission to "/opt/sonarqube/"****

```bash
sudo chown sonar:sonar /opt/sonarqube/ -R
```


### STEP 05: Configure SonarQube 

Now, Let's head-over to "**sonar.properties**" configuration file and do the following changes

```bash
sudo vim /opt/sonarqube/conf/sonar.properties
```


**UnComment and type PostgreSQL database username and password that we've created at privous step.**

We are using "localhost" as db host since we've installed postgreSQl on same server.

```bash
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube
sonar.search.javaOpts=-Xmx512m -Xms512m -XX:+HeapDumpOnOutOfMemoryError
```


`########### OPTIONAL USE ONLY #############
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube
sonar.web.host=127.0.0.1
sonar.web.port=9000
sonar.web.javaAdditionalOpts=-server
sonar.search.javaOpts=-Xmx512m -Xms512m -XX:+HeapDumpOnOutOfMemoryError
sonar.log.level=INFO
sonar.path.logs=logs
###########################################`

### STEP 06: Configure Systemd Service For SonarQube

Now, Create a startup script for SonarQube service that start at system boots

Create a systemd service file for SonarQube to be run at system startup. 

```bash
vim /etc/systemd/system/sonarqube.service
```


Add below content to the "sonarqube.service" file.

```bash
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always

LimitNOFILE=65536
LimitNPROC=4096


[Install]
WantedBy=multi-user.target
```

**Enable & Start SonarQube Service**

```bash
systemctl daemon-reload 
systemctl enable sonarqube.service
systemctl start sonarqube.service
systemctl status -l sonarqube.service
```


Verify Service status using this command 

```bash
netstat -tulpena  | grep 9000
```


##### OPTIONAL : 

Additionally you may required to modify some entries related to  elasticsearch and JVM options, Therefore SonarQube using elastciseach and JVM options.

`/opt/sonarqube/elasticsearch/config/elasticsearch.yml`

`/opt/sonarqube/elasticsearch/config/log4j2.properties`

### STEP 07: Configure NGINX Reverse Proxy For SonarQube

**Install NGINX Package**

Now we need to expose our SonarQube server into outside as it is listening only on localhost. Therefore we are creating a Nginx reverse proxy to redirect outside traffic into the SonarQube.  

```bash
apt-get install nginx
```


**Create NGINX Configuration File For SonarQube**

sudo vim /etc/nginx/sites-available/sonarqube

Copy and paste this vertual-host server block and change "server_name" entry as you required.

```bash
server{
    listen      80;
    server_name sonarqube.da.com;

    access_log  /var/log/nginx/sonar.access.log;
    error_log   /var/log/nginx/sonar.error.log;

    proxy_buffers 16 64k;
    proxy_buffer_size 128k;

    location / {
        proxy_pass  http://127.0.0.1:9000;
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        proxy_redirect off;

        proxy_set_header    Host            $host;
        proxy_set_header    X-Real-IP       $remote_addr;
        proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header    X-Forwarded-Proto http;
    }
}
```



**Check ENGINX configurations**

`nginx -t`

**Enable & Restart Nginx Service**

```bash
systemctl enable nginx.service 
systemctl restart nginx.service
```


SonarQube stores their service logs under "/opt/sonarqube/logs" directory. You may need those log files in case of troubleshooting purpose.

**Troubleshooting Tips : Log Paths**

`/opt/sonarqube/logs/es.log` 

`/opt/sonarqube/logs/sonar.log` 

`/opt/sonarqube/logs/web.log` 


### STEP 08: Firewall Configuration

**Allow TCP ports 9000, 9001, 80 through the firewall**

```bash
sudo ufw allow 80,9000,9001/tcp

sudo ufw status
```


### STEP 09: Access SonarQube Through Web Browser

Now, SonarQube installation & configuration has been completed. It's time to access web console through the web browser.

Provide the default administrator account username and password as admin / admin

**Default Username: admin**

**Default Password: admin**

> http://172.25.10.10/ OR http://YOUR-SERVER-IP


<img src="\images\SonarQube-Ubuntu\1.png" width="100%">
<img src="\images\SonarQube-Ubuntu\2.png" width="100%">

**SonarQube initial configuration has been completed. 
In the next tutorial, I will show you how to integrate and analyze your project code on SonarQube with Jenkins server and GitLab. And analysis of code deployments real-time.**

If you need further clarification, Please ask on YouTube video comment section.

<img src="\images\SonarQube-Ubuntu\3.png" width="100%">
<img src="\images\SonarQube-Ubuntu\4.png" width="100%">
<img src="\images\SonarQube-Ubuntu\5.png" width="100%">
<img src="\images\SonarQube-Ubuntu\6.png" width="100%">