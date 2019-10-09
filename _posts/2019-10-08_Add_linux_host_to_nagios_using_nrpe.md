
# How to Add Linux Host to Nagios Monitoring Server Using NRPE Plugin

## On Client Host 

#### STEP 01: How To Install NRPE v3 From YUM Repository

A. Install Prerequisites

```bash
yum install -y gcc glibc glibc-common openssl openssl-devel perl wget
```

B. Install Latest EPEL Repository And System Updates

Install Latest EPEL YUM Repository

```bash
yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```

```bash
yum -y update
```

C. Install (NRPE v3) Nagios Remote Plugin Executor
 
```bash
yum -y install nrpe
```

D. Update NRPE Configuration File

Add the private IP address of the Nagios server 

allowed_hosts=<Nagios_Server_IP>

```bash
vim /etc/nagios/nrpe.cfg
```


```bash
allowed_hosts=127.0.0.1,172.25.10.50
```

Define here your commands that you want to execute on remote host.
Comment/UnComment According To Your Requirements

```bash
command[check_users]=/usr/lib64/nagios/plugins/check_users -w 5 -c 10
command[check_load]=/usr/lib64/nagios/plugins/check_load -r -w .15,.10,.05 -c .30,.25,.20
command[check_hda1]=/usr/lib64/nagios/plugins/check_disk -w 20% -c 10% -p /dev/hda1
command[check_zombie_procs]=/usr/lib64/nagios/plugins/check_procs -w 5 -c 10 -s Z
command[check_total_procs]=/usr/lib64/nagios/plugins/check_procs -w 150 -c 200
```


D. Allow NRPE service Port Through the Firewall
Allow TCP port 5666 through the firewall to be able to listening through the TCP port 5666


```bash
[root@nagios /]# firewall-cmd --permanent --add-port=5666/tcp
[root@nagios /]# firewall-cmd --reload
```

F. Enable NRPE Service To Start at Boot

```bash
systemctl enable nrpe.service
systemctl restart nrpe.service
systemctl status nrpe.service
```

# STEP 02: Installing Nagios Plugins From YUM Repository

A. Install Prerequisites

```bash
yum install -y gcc glibc glibc-common make gettext automake autoconf wget openssl-devel net-snmp net-snmp-utils epel-release
yum install -y perl-Net-SNMP
```
B. Install Nagios Plugins

```bash
yum -y install nagios-plugins-all
```

C. Copy plugins if not available in the directory.
 

```bash
cp /usr/lib64/nagios/plugins/check_* /usr/local/nagios/libexec/
```

D. Restart Nagios, Apache and NRPE Servers

```bash
systemctl restart nagios httpd nrpe
```
# STEP 03: Define New Hosts & Services 
## On Nagios Server

Create the directory that will store the configuration file for each server that we will monitor.

```bash
sudo mkdir /usr/local/nagios/etc/servers
```

Create a new configuration file for each of the remote hosts that we want to monitor 

```bash
vim /usr/local/nagios/etc/servers/cl1.cfg
```

```bash
# Host configuration file
define host {
        use                          linux-server
        host_name                    cl1
        alias                        Ubuntu Host
        address                      172.25.10.100
        register                     1
}

define service {
      host_name                       cl1
      service_description             PING
      check_command                   check_ping!100.0,20%!500.0,60%
      max_check_attempts              2
      check_interval                  2
      retry_interval                  2
      check_period                    24x7
      check_freshness                 1
      contact_groups                  admins
      notification_interval           2
      notification_period             24x7
      notifications_enabled           1
      register                        1
}

define service {
      host_name                       cl1
      service_description             Check Users
      check_command           check_local_users!20!50
      max_check_attempts              2
      check_interval                  2
      retry_interval                  2
      check_period                    24x7
      check_freshness                 1
      contact_groups                  admins
      notification_interval           2
      notification_period             24x7
      notifications_enabled           1
      register                        1
}

define service {
      host_name                       cl1
      service_description             Local Disk
      check_command                   check_local_disk!20%!10%!/
      max_check_attempts              2
      check_interval                  2
      retry_interval                  2
      check_period                    24x7
      check_freshness                 1
      contact_groups                  admins
      notification_interval           2
      notification_period             24x7
      notifications_enabled           1
      register                        1
}

define service {
      host_name                       cl1
      service_description             Check SSH
      check_command                   check_ssh
      max_check_attempts              2
      check_interval                  2
      retry_interval                  2
      check_period                    24x7
      check_freshness                 1
      contact_groups                  admins
      notification_interval           2
      notification_period             24x7
      notifications_enabled           1
      register                        1
}

define service {
      host_name                       cl1
      service_description             Total Process
      check_command                   check_local_procs!250!400!RSZDT
      max_check_attempts              2
      check_interval                  2
      retry_interval                  2
      check_period                    24x7
      check_freshness                 1
      contact_groups                  admins
      notification_interval           2
      notification_period             24x7
      notifications_enabled           1
      register                        1
}
```

```bash
vim /usr/local/nagios/etc/nagios.cfg
```

```bash
# Definitions for monitoring the local (Linux) host
cfg_file=/usr/local/nagios/etc/servers/cl1.cfg
```

Now restart Nagios server to reflect the configuration changes.

```bash
systemctl restart nagios.service
```

# STEP 04: Add New Service 

#### On Nagios monitoring server:

A. Edit the file commands.cfg and add the lines below.

```bash
vim /usr/local/nagios/etc/objects/commands.cfg
```

```bash
#'check_dns' command definition
define command {
        command_name    check_dns
        command_line       $USER1$/check_dns -H $HOSTADDRESS$ -s $ARG1$ 
}
```

OR

```bash
#'check_dns' command definition
define command {
        command_name    check_dns
        command_line    $USER1$/check_dns -H digitalave.github.io -s 8.8.8.8 -t 15 | sed  -e "s/returns.*//g" 
}

```

$ARG1$ is the argument you will parse into in the configuration
$HOSTADDRESS$ is your host on Nagios monitoring.


B. Verify NRPE Daemon

> /usr/local/nagios/libexec/check_nrpe -H <remote_linux_ip_address>

OR

> /usr/local/nagios/libexec/check_dns -H digitalave.github.io -s 8.8.8.8 -t 15 | sed  -e "s/returns.*//g" 

C. Edit Host & Service  Configuration File for CL1 Host

Next, put the line below into service check configuration file

```bash
vim /usr/local/nagios/etc/servers/cl1.cfg
```



```bash
define service {
      host_name                       cl1
      service_description             DNS
      check_command                   check_dns
      max_check_attempts              2
      check_interval                  2
      retry_interval                  2
      check_period                    24x7
      check_freshness                 1
      contact_groups                  admins
      notification_interval           2
      notification_period             24x7
      notifications_enabled           1
      register                        1
}
```

D. Verify Nagios Configs and Check For Error

```bash
/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```


Restart your Nagios service now

```bash
systemctl restart nagios
```

#### ON Client:Remote Host (the machine you want to monitor)

E. Configure NRPE Agent Configuration File

```bash
vim /etc/nagios/nrpe.cfg
```


Uncomment This Line

```bash
command[check_dns]=/usr/lib64/nagios/plugins/check_disk $ARG1$
```


Restart NRPE service

```bash
systemctl restart  nrpe
```

F. Log Into Nagios Web Console

