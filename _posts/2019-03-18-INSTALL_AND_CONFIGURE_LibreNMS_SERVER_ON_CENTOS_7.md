---
layout: post
authors: [dimuthu_daundasekara]
title: 'INSTALL AND CONFIGURE LibreNMS SERVER ON CENTOS 7'
image: images/librenms/LibreNMS.png
tags: [LibreNMS, Server Monitoring]
category: Testing
comments: true
---

<img src="/images/librenms/LibreNMS.png" width="100%">


## INSTALL AND CONFIGURE LibreNMS SERVER ON CENTOS 7

### What is LibreNMS ?

LibreNMS is an opensource monitoring tool for servers and network hardware.
Based on Nginx or Apache, MySQL, and PHP.
LibreNMS can discover network using UDP, FDP, LLDP, OSPF, BGP, SNMP and ARP portocols.Collects monitoring metrices via SNMP protocol.



#### Prerequisite:-

1.Root login

2.CenstOS 7

3.EPEL repository

### STEPS

**STEP 1:-Install Required Packages.**

**STEP 2:-Install NginX Web Server.**

**STEP 3:-Install and Configure MariaDB.**

**STEP 4:-Download and Configure LibreNMS.**

**STEP 5:-LibreNMS Web-based Installation.**

**STEP 6:-Firewall and SELinux Configuration.**

**STEP 7:-Install FPing and SNMP.**

**STEP 8:-Set Permission.**

**STEP 9:-LibreNMS Web Installation.**


### STEP 1:- Install Required Packages

Configure EPEL Repository.

```bash
[root@LibreNMS ~]# rpm -Uvh https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-7-11.noarch.rpm
```

<img src="/images/librenms/image001.png" width="100%">



Configure Webtatic Repository.

```bash
[root@LibreNMS ~]# rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
```


<img src="/images/librenms/image002.png" width="100%">

Install some packages including ImageMagic, rrdtool, git, snmp, nmap, fping and more.


```bash
[root@LibreNMS ~]# yum install -y cronie fping git ImageMagick jwhois mtr net-snmp net-snmp-utils nmap rrdtool MySQL-python python-memcached
```

### STEP 2:- Install Nginx Web Server

```bash
[root@LibreNMS ~]# yum install -y nginx
[root@LibreNMS ~]# systemctl enable nginx
[root@LibreNMS ~]# systemctl start  nginx
```


<img src="/images/librenms/image003.png" width="100%">

### STEP 3:- Install and Configure PHP-FPM

Install PHP-FPM version 7 for CentOS7 from Webtatic repository. Webtatic repository has been installed on previous step.


```bash
[root@LibreNMS ~]# yum install -y composer php72w php72w-cli php72w-common php72w-curl php72w-fpm php72w-gd php72w-mbstring php72w-mysqlnd php72w-process php72w-snmp php72w-xml php72w-zip
```


Update the PEAR (PHP Extension and Application Repository) repository.


```bash
[root@LibreNMS ~]# yum install php72w-pear.noarch
[root@LibreNMS ~]# pear channel-update pear.php.net
[root@LibreNMS ~]# pear install Net_IPv4-1.3.4
[root@LibreNMS ~]# pear install Net_IPv6-1.2.2b2
```


<img src="/images/librenms/image004.png" width="100%">

Set default timezone in the php.ini file. un-comment and edit look like below.

> date.timezone = Asia/Colombo

> cgi.fix_pathinfo=0


Now, Set PHP-FPM to running under the ‘sock’ file instead of the server port.

```bash
[root@LibreNMS ~]# vim /etc/php-fpm.d/www.conf
```


Change ‘listen’ port line to the sock file below.

```bash
touch /var/run/php-fpm/php7.2-fpm.sock chmod 777
/var/run/php-fpm/php7.2-fpm.sock
```


```bash
listen = /var/run/php-fpm/php7.2-fpm.sock
```


Ucomment the ‘listen’ owner, group and the permission of the sock file.

```bash
listen.owner = nginx
listen.group = nginx
listen.mode = 0660
```


```bash
[root@LibreNMS ~]# systemctl start php-fpm.service 
[root@LibreNMS ~]# systemctl enable php-fpm.service
```


Now, PHP-FPM should running under the sock file.

```bash
[root@LibreNMS ~]# netstat  -pl | grep php
```


<img src="/images/librenms/image005.png" width="100%">

### STEP 3:- Install and Configure MariaDB

**Install MariaDB**

```bash
[root@LibreNMS ~]# yum install -y mariadb mariadb-server 
[root@LibreNMS ~]# systemctl start mariadb.service 
[root@LibreNMS ~]# systemctl enable mariadb.service
```


<img src="/images/librenms/image006.png" width="100%">

> Set root password? [Y/n] Y
> 
> Remove anonymous users? [Y/n] Y
> 
> Disallow root login remotely? [Y/n] Y
> 
> Remove test database and access to it? [Y/n] Y
> 
> Reload privilege tables now? [Y/n] Y


Create a new database and a user for LibreNMS.

Create database named “librenms” and a user named “librenms” with password **

```bash
[root@LibreNMS ~]# mysql -u root –p
MariaDB [(none)]> CREATE DATABASE librenms CHARACTER SET utf8 COLLATE utf8_unicode_ci;
MariaDB [(none)]> CREATE USER 'librenms'@'localhost' IDENTIFIED BY '<PASSWORD>';
MariaDB [(none)]> GRANT ALL PRIVILEGES ON librenms.* TO 'librenms'@'localhost';
MariaDB [(none)]> FLUSH PRIVILEGES;
MariaDB [(none)]> quit;
```



<img src="/images/librenms/image007.png" width="100%">

Edit /etc/my.cnf file and add new configuration.

Paste below additional configuration under the ‘[mysqld]’ section.

> innodb_file_per_table=1
>
> sql-mode=””
>
> lower_case_table_names=0



<img src="/images/librenms/image008.png" width="100%">

```bash
[root@LibreNMS ~]# systemctl restart mariadb.service
```


### STEP 4:- Download and Configure LibreNMS

Create new system user named ‘librenms’ and define the home directory under ‘/opt/librenms’ and add ‘librenms’ user to ‘nginx’ group.

```bash
[root@LibreNMS ~]# useradd librenms -d /opt/librenms -M -r
[root@LibreNMS ~]# usermod -a -G librenms nginx
```

Clone LibreNMS into /opt/librenms directory.


```bash
[root@LibreNMS opt]# cd /opt/
[root@LibreNMS opt]# git clone https://github.com/librenms/librenms.git librenms
```



**This Step is optional…**

Create new directories for LibreNMS logs and rrd files.
```bash
[root@LibreNMS opt]# mkdir -p /opt/librenms/{logs,rrd}
[root@LibreNMS opt]# chmod 755 /opt/librenms/rrd/
```


<img src="/images/librenms/image009.png" width="100%">


<img src="/images/librenms/image010.png" width="100%">


Change ownership of all files and directories under ‘/opt/librenms’ to the ‘librenms’ user and group.


```bash
[root@LibreNMS opt]# chown -R librenms:librenms /opt/librenms/
```


<img src="/images/librenms/image011.png" width="100%">



**Configure LibreNMS virtual host**

LibreNMS is a Web-based application and we using Nginx web server to host it.
Create a new virtual host file user ‘librenms.conf’ under Nginx ‘conf.d’ directory.

```bash
[root@LibreNMS ~]# vim /etc/nginx/conf.d/librenms.conf
```


Paste configuration below.


```bash
server {
 listen      80;
 server_name librenms.orelit.com;
 server_name 192.168.100.10;
 root        /opt/librenms/html;
 index       index.php;

 charset utf-8;
 gzip on;
 gzip_types text/css application/javascript text/javascript application/x-javascript image/svg+xml text/plain text/xsd text/xsl text/xml image/x-icon;
 location / {
  try_files $uri $uri/ /index.php?$query_string;
 }
 location /api/v0 {
  try_files $uri $uri/ /api_v0.php?$query_string;
 }
 location ~ \.php {
  include fastcgi.conf;
  fastcgi_split_path_info ^(.+\.php)(/.+)$;
  fastcgi_pass unix:/var/run/php-fpm/php7.2-fpm.sock;
 }
 location ~ /\.ht {
  deny all;
 }
}
```


**Test Nginx configuration.**

```bash
[root@LibreNMS ~]# nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```


```bash
[root@LibreNMS ~]# systemctl restart nginx.service
```



**NOTE:**  If this is the only site you are hosting on this server. Then should need to disable default site. Delete the ‘server’ section from /etc/nginx/nginx.conf



### STEP 6:- Firewall and SELinux Configuration

Allow Access through FirewallD:-

```bash
[root@LibreNMS ~]# firewall-cmd –permanent --zone public --add-service=http
[root@LibreNMS ~]# firewall-cmd --permanent --zone public --add-service=https
[root@LibreNMS ~]# firewall-cmd --permanent --zone public --add-port=161/udp
[root@LibreNMS ~]# firewall-cmd –reload
```


SELinux Confguration:-

```bash
[root@LibreNMS ~]# yum install policycoreutils-python
[root@LibreNMS ~]# semanage fcontext -a -t httpd_sys_content_t '/opt/librenms/logs(/.*)?'
[root@LibreNMS ~]# semanage fcontext -a -t httpd_sys_rw_content_t '/opt/librenms/logs(/.*)?'
[root@LibreNMS ~]# restorecon -RFvv /opt/librenms/logs/
[root@LibreNMS ~]# semanage fcontext -a -t httpd_sys_content_t '/opt/librenms/rrd(/.*)?'
[root@LibreNMS ~]# semanage fcontext -a -t httpd_sys_rw_content_t '/opt/librenms/rrd(/.*)?'
[root@LibreNMS ~]# restorecon -RFvv /opt/librenms/rrd/
[root@LibreNMS ~]# semanage fcontext -a -t httpd_sys_content_t '/opt/librenms/storage(/.*)?'
[root@LibreNMS ~]# semanage fcontext -a -t httpd_sys_rw_content_t '/opt/librenms/storage(/.*)?'
[root@LibreNMS ~]# restorecon -RFvv /opt/librenms/storage/
[root@LibreNMS ~]# semanage fcontext -a -t httpd_sys_content_t '/opt/librenms/bootstrap/cache(/.*)?'
[root@LibreNMS ~]# semanage fcontext -a -t httpd_sys_rw_content_t '/opt/librenms/bootstrap/cache(/.*)?'
[root@LibreNMS ~]# restorecon -RFvv /opt/librenms/bootstrap/cache/
[root@LibreNMS ~]# setsebool -P httpd_can_sendmail=1
[root@LibreNMS ~]# setsebool -P httpd_execmem 1
```



### STEP 7:- Configure FPing, SNMP and Cron Job


Configure FPing:
Create the file http_fping.tt with the following contents.

```bash
[root@librenms etc]# vim http_fping.tt
```

```bash
require {
type httpd_t;
class capability net_raw;
class rawip_socket { getopt create setopt write read };
}

#============= httpd_t ==============
allow httpd_t self:capability net_raw;
allow httpd_t self:rawip_socket { getopt create setopt write read };
```


**Run These Commands:**

```bash
[root@librenms etc]# checkmodule -M -m -o http_fping.mod /etc/http_fping.tt
[root@librenms etc]# semodule_package -o http_fping.pp -m http_fping.mod
[root@librenms etc]# semodule -i http_fping.pp
```



**Configure SNMP:**


```bash
[root@librenms ~]# cp /opt/librenms/snmpd.conf.example /etc/snmp/snmpd.conf
```


```bash
[root@librenms ~]# vim /etc/snmp/snmpd.conf
“com2sec readonly default public”
```


```bash
[root@librenms ~]# curl -o /usr/bin/distro https://raw.githubusercontent.com/librenms/librenms-agent/master/snmp/distro
[root@librenms ~]# chmod +x /usr/bin/distro
[root@librenms ~]# systemctl enable snmpd
[root@librenms ~]# systemctl restart snmpd
Configure Cron Job:
[root@librenms ~]# cp /opt/librenms/librenms.nonroot.cron /etc/cron.d/librenms
Create Log Rotation Task
[root@librenms ~]# cp /opt/librenms/misc/librenms.logrotate /etc/logrotate.d/librenms
```



### STEP 8:- Set Permission


```bash
[root@librenms ~]# chown -R librenms:librenms /opt/librenms
[root@librenms ~]# setfacl -d -m g::rwx /opt/librenms/rrd /opt/librenms/logs /opt/librenms/bootstrap/cache/ /opt/librenms/storage/
[root@librenms ~]# setfacl -R -m g::rwx /opt/librenms/rrd /opt/librenms/logs /opt/librenms/bootstrap/cache/ /opt/librenms/storage/
```



### STEP 9:- LibreNMS Web Installation


**NOTE:** If following errors occurred, do below steps.

A.


<img src="/images/librenms/image012.png" width="100%">


```bash
[root@LibreNMS ~]# cd /opt/librenms/
[root@LibreNMS librenms]# ./scripts/composer_wrapper.php install --no-dev
```

B.


<img src="/images/librenms/image013.png" width="100%">


```bash
chown -R librenms:librenms /opt/librenms/bootstrap/cache /opt/librenms/storage /opt/librenms/storage/framework/sessions /opt/librenms/storage/framework/views /opt/librenms/storage/framework/cache /opt/librenms/logs
setfacl -R -m g::rwx /opt/librenms/bootstrap/cache /opt/librenms/storage /opt/librenms/storage/framework/sessions /opt/librenms/storage/framework/views /opt/librenms/storage/framework/cache /opt/librenms/logs
setfacl -d -m g::rwx /opt/librenms/bootstrap/cache /opt/librenms/storage /opt/librenms/storage/framework/sessions /opt/librenms/storage/framework/views /opt/librenms/storage/framework/cache /opt/librenms/logs
usermod -a -G librenms nginx
```



C.

<img src="/images/librenms/image014.png" width="100%">


```bash
semanage fcontext -a -t httpd_sys_content_t '/opt/librenms/bootstrap/cache(/.*)?'
semanage fcontext -a -t httpd_sys_rw_content_t '/opt/librenms/bootstrap/cache(/.*)?'
restorecon -RFvv /opt/librenms/bootstrap/cache
semanage fcontext -a -t httpd_sys_content_t '/opt/librenms/storage(/.*)?'
semanage fcontext -a -t httpd_sys_rw_content_t '/opt/librenms/storage(/.*)?'
restorecon -RFvv /opt/librenms/storage
semanage fcontext -a -t httpd_sys_content_t '/opt/librenms/storage/framework/sessions(/.*)?'
semanage fcontext -a -t httpd_sys_rw_content_t '/opt/librenms/storage/framework/sessions(/.*)?'
restorecon -RFvv /opt/librenms/storage/framework/sessions
semanage fcontext -a -t httpd_sys_content_t '/opt/librenms/storage/framework/views(/.*)?'
semanage fcontext -a -t httpd_sys_rw_content_t '/opt/librenms/storage/framework/views(/.*)?'
restorecon -RFvv /opt/librenms/storage/framework/views
semanage fcontext -a -t httpd_sys_content_t '/opt/librenms/storage/framework/cache(/.*)?'
semanage fcontext -a -t httpd_sys_rw_content_t '/opt/librenms/storage/framework/cache(/.*)?'
restorecon -RFvv /opt/librenms/storage/framework/cache
semanage fcontext -a -t httpd_sys_content_t '/opt/librenms/logs(/.*)?'
semanage fcontext -a -t httpd_sys_rw_content_t '/opt/librenms/logs(/.*)?'
restorecon -RFvv /opt/librenms/logs
```



**Access the LibreNMS Installation Page:-**

http://<IP_OR_FQDN>/install.php

**Check PHP Module Availability.**


<img src="/images/librenms/image015.png" width="100%">


**Database Configuration.**

DB User: librenms

DB Name: librenms

DB Password: The Password given for the librenms database


<img src="/images/librenms/image016.png" width="100%">


**Import MySQL Databse.**
Wait untill importing completed. Make sure NO-ERRORS.


<img src="/images/librenms/image017.png" width="100%">


**Create LibreNMS Web Admin Login User.**


<img src="/images/librenms/image018.png" width="100%">

**Generate Config File.**

<img src="/images/librenms/image019.png" width="100%">


<img src="/images/librenms/image020.png" width="100%">


If this error occurred, you have to manually create config.php file under /opt/librenms/ directory.

Copy php code and create config.php file under /opt/librenms directory.

```bash
[root@LibreNMS ~]# vim /opt/librenms/config.php
```


Change ownership of config.php file to librenms user and group.

```bash
[root@LibreNMS ~]# chown librenms:librenms /opt/librenms/config.php
```


<img src="/images/librenms/image021.png" width="100%">


<img src="/images/librenms/image022.png" width="100%">


### Post Installation Setup:-

```bash
[root@librenms librenms]# vim /opt/librenms/config.php
$config[‘fping’] = “/usr/sbin/fping”;
```


**Finally Validate:-**

```bash
[root@librenms librenms]# ./validate.php
```


<img src="/images/librenms/image013.png" width="100%">


**Little Request:**

> I appreciate you guys taking the time in reading my post. Please check out my YouTube channel and please subscribe for more as it'll help me loads.


<a href="https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ/featured?view_as=subscriber" target="_blank">https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ/featured?view_as=subscriber</a>











