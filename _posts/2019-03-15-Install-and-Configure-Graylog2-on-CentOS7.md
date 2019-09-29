---
layout: post
authors: [dimuthu_daundasekara]
title: 'INSTALL AND CONFIGURE GrayLog2 SERVER ON CENTOS 7'
image: /images/graylog_ins/graylog.png
tags: [Graylog, Logs, Log Monitoring]
category: Spring
comments: true
---

<img src="/images/graylog_ins/graylog.png" width="100%">

# INSTALL AND CONFIGURE GrayLog2 SERVER ON CENTOS 7

Graylog is an open source log management tool. It can use for collect, index and analyze remote machine logs centrally.

## Components: -

**MongoDB - Stores the configuration and meta information.**

**Elasticsearch - Store the log messages and offers searching facility which are coming from Graylog server. Elasticsearch does indexing of data.**

**Graylog Server - Collect logs coming from various inputs and provide Web based interface to manage those logs.**

## Pre-requisites: -

Elasticsearch is based on Java
Install Oracle Java / OpenJDK

```bash
[root@graylog /]# rpm -Uvh jdk-8u161-linux-x64.rpm
```

<img src="/images/graylog_ins/image001.png" width="100%">

<img src="/images/graylog_ins/image002.png" width="100%">

## Install Elasticsearch: -

Elasticsearch is an open source tool. Which provides distributed search, indexing and analytics using RESTful web interface. Elasticsearch stores all the log sent by Graylog server inputs and displays the messages.

<img src="/images/graylog_ins/image004.png" width="100%">

Download and install public singing key.

<img src="/images/graylog_ins/image006.png" width="100%">

```bash
[root@graylog /]# rpm --import https://artifacts.elastic.co/GPG-KEY-elasticsearch
[root@graylog /]# vim /etc/yum.repos.d/elasitcsearch.repo
[elasticsearch-5.x]
name=Elasticsearch repository for 5.x packages
baseurl=https://artifacts.elastic.co/packages/5.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md
```

<img src="/images/graylog_ins/image008.png" width="100%">




```bash
[root@graylog /]# yum install -y elasticsearch
```


```bash
[root@graylog /]# systemctl enable elasticsearch
[root@graylog /]# systemctl start elasticsearch
[root@graylog /]# systemctl daemon-reload
```

## Configure Elasticsearch: -

Elasticsearch configuration files can be found in /etc/elasticsearch/ directory.

**logging.yml – manages the logging of elasticsearch**

**elasticsearch.yml – main configuration file**

Log files stores in /var/log/elasticsearch/

By default

Bind to all network interfaces 0.0.0.0
HTTP traffic Listen on port 9200 – 9300
Internal node to node communication on port 9300 – 9400

Do the following changes to listen on specific IP.

```bash
[root@graylog /]# vim /etc/elasticsearch/elasticsearch.yml
network.host: 192.168.100.10
```


The cluster.name is used to discover and auto-join other nodes. Use unique cluster name to avoid auto-join with other Elasticsearch server clusters.

```bash
cluster.name: graylog
```

Disable dynamic scripts to avoid remote execution

```bash
script.inline: false
script.indexed: false
script.file: false
```

```bash
[root@graylog /]# systemctl restart  elasticsearch.service
```

Elasticsearch now starts to listen on port 9200 for HTTP requests. Use this command to check whether it is working.

[root@graylog /]# curl -X GET 'http://192.168.100.10:9200'

<img src="/images/graylog_ins/image010.png" width="100%">

To check the Elasticsearch server’s health. Status should be as “green” to work properly.

```bash
[root@graylog ~]# curl -XGET 'http://192.168.100.10:9200/_cluster/health?pretty=true'
```

<img src="/images/graylog_ins/image011.png" width="100%">

Install MongoDB: -

Create MongoDB yum repository.

```bash
[root@graylog /]# vi /etc/yum.repos.d/mongodb-org-3.2.repo
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

<img src="/images/graylog_ins/image013.png" width="100%">


```bash
root@graylog ~]# yum install -y mongodb-org
```

`
SELinux Configuration for MongoDB

```bash
root@graylog ~]# yum -y install policycoreutils-python
```

SELinux to allow MongoDB to Start.

```bash
[root@graylog ~]# semanage port -a -t mongod_port_t -p tcp 27017
```

Enable and Start MongoDB Service

```bash
[root@graylog /]# systemctl enable mongod.service
[root@graylog /]# systemctl start mongod.service
```

## Install Graylog2: -

Graylog-server accepts and process the log messages receiving from various inputs and display data through Graylog web interface

```bash
[root@graylog /]# rpm -Uvh https://packages.graylog2.org/repo/packages/graylog-2.4-repository_latest.rpm
[root@graylog /]# yum install graylog-server
```
<img src="/images/graylog_ins/image015.png" width="100%">

<img src="/images/graylog_ins/image017.png" width="100%">

<img src="/images/graylog_ins/image019.png" width="100%">

<img src="/images/graylog_ins/image021.png" width="100%">

<img src="/images/graylog_ins/image023.png" width="100%">

<img src="/images/graylog_ins/image025.png" width="100%">

<img src="/images/graylog_ins/image027.png" width="100%">

<img src="/images/graylog_ins/image029.png" width="100%">

<img src="/images/graylog_ins/image031.png" width="100%">

<img src="/images/graylog_ins/image033.png" width="100%">

<img src="/images/graylog_ins/image035.png" width="100%">

<img src="/images/graylog_ins/image037.png" width="100%">

**Little Request:**

> I appreciate you guys taking the time in reading my post. Please check out my YouTube channel and please subscribe for more as it'll help me loads.


<a href="https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ/featured?view_as=subscriber" target="_blank">https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ/featured?view_as=subscriber</a>

[<img src="/images/Docker-Installation/sub.gif">](https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ?sub_confirmation=1) 

[![Foo](/images/Docker-Installation/sub.gif)](https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ?sub_confirmation=1)