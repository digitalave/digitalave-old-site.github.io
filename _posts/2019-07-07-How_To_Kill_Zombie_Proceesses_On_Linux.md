---
layout: post
authors: [dimuthu_daundasekara]
title: 'How To Kill Zombie Processes on Linux'
image: /images/zombie_process/Zombie_N_N.jpg
tags: [pfSense, Firewall, Captive Portal, WIFI]
category: Spring
comments: true
---



# Install & Configure ELK Stack On CentOS7

<img src="/images/zombie_process/Zombie_N_N.jpg" width="100%">

### How To Kill Zombie Processes on Linux

#### What is a Zombie Process ?

Normally, When we launch a program, It starts their task & once task is over, Its end that process. Once process ended, It hos to be removed from the process table.

Sometimes, If programs are not programmed well enough to remove it's child processes (Zombies). Because of this, some of these child zombie processes are reside in the process table even after it has completed its execution.

So, These process we called Zombie processes.

#### STEPS:

`Run a Program >> Creates a Parent Process with Child Processes >> They utilize System's resources >> Complete Child Process Task >> Receive EXIT system call >> Die Child Process >> EXIT system call read by Parent >> Finally Child Process removed from Process table.`


If Parent failed to read the EXIT system call from the child process, The child processes are already exists on the process table even though that child process are already dead.

#### Are they Harmful ?

No, But, If there are lot of Zombie process which will stored/collected in the RAM.

#### How To Find Zombie Processes ? 

```bash
ps aux |grep Z
```


OR

```bash
ps aux |grep "defunct"
```


OR

```bash
ps -A -ostat,ppid | awk '/[zZ]/ && !a[$2]++ {print $2}'
```


#### How To Kill Zombie Process ?

```bash
kill -s SIGCHLD <PID OF PARENT>
```


OR

```bash
kill -9 $(ps -A -ostat,ppid | awk '/[zZ]/ && !a[$2]++ {print $2}')
```


**Note: You Can Use This For Rsync Zombie Processes As Well.**

#### Alternatively, You can use PKILL command like this way.

```bash
kill -9 $(ps -Z | grep "<PROCESS_EXACT_NAME>" | awk '{print $2}')
```


OR

```bash
pkill --signal --SIGCHLD rsync
```

[<img src="/images/Docker-Installation/sub.gif">](https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ?sub_confirmation=1) 

[![Foo](/images/Docker-Installation/sub.gif)](https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ?sub_confirmation=1)