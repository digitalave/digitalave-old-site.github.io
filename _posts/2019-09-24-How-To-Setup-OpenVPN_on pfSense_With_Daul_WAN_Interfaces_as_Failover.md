---
layout: post
authors: [dimuthu_daundasekara]
title: 'How To Setup an OpenVPN on pfSense with Dual-WAN Interfaces as Fail-over'
image: /images/dual-wan-pf-vpn/Dual-WAN-OVPN-pfSense.jpg
tags: [pfsense, OpenVPN,Failover Firewall]
category: Spring
comments: true
---

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/embed/-uwONmaLwwg' frameborder='0' allowfullscreen></iframe></div>


### How To Setup an OpenVPN on pfSense with Dual-WAN Interfaces as Fail-over 

#### Introduction:

**OpenVPN is really useful to access to your office when you are out of the office.**

**In this tutorial I'm going to  show you how to setup your own VPN connection using with OpenVPN service on pfSense firewall.
But, the really special thing is what would happens if your Internet link goes down?**


**Now....  Here's the solution...**

**But, How ???** 

To achieve this task you may need to have two WAN interfaces which attached to  your pfSense firewall.

You will have a uninterrupted VPN access, After the completion of this Multi-WAN OpenVPN setup.

##### STEP 01: SETUP OpenVPN

Now head over to "**VPN**" tab and select "**OpenVPN**" from  the drop  down.

Click & open  "**Wizard**"

<img src="/images/dual-wan-pf-vpn/2.png" width="100%">

1. Select "**Local User Access**" as the Authentication Backed Type.

Click next & move onto "**Create CA Certificate**" section.

Create New CA Certificate with **Descriptive Certificate Name**, **Country Code**, **State of Province**, **City** and **Organization**.

<img src="/images/dual-wan-pf-vpn/3.png" width="100%">

Then Move onto next step.

2. Now, We have to create a new server certificate same as we done earlier.

<img src="/images/dual-wan-pf-vpn/4.png" width="100%">

Move to next step  after that.

3. Now We need to modify "**General Settings**" for the OpenVPN

* As the first option, we need to select an **interface** wish to configure.

For now, I'm going  to select WAN1 interface.

* You may no need modify rest of the other fields. 

<img src="/images/dual-wan-pf-vpn/5.png" width="100%">

But, At this time I'm going  to  change "**Encryption algorithem**"

* Now head over to "**Tunnel Settings**" section.

Add "**Tunnel Network**" address. This is a virtual network address which provide an IP address to cleint. It helps private communication between OpenVPN server & client. Provide a network address which  is not exists on your network.

* Then, Add network  address of your "**Local Area Network**". 

This network should be the network that you wants  to access via OpenVPN.

* Simply enable "**Inter Clint Communication**" to  enable the communication between other OpenVPN clients.

* You may  need to  provide "**domain name**" and "DNS server's for the "**Client Settings**" section

* As the final step, You need to  "**Configure Firewall**" to allow connection through the tunnel  network.

<img src="/images/dual-wan-pf-vpn/6.png" width="100%">

<img src="/images/dual-wan-pf-vpn/7.png" width="100%">
And click finish button.

OK, OpenVPN Server configuration has been completed so far.

But, We need to create two firewall NAT Port forwarding rules since we are going to  use 2 Wan interfaces.

##### STEP 02: Create Port Forwarding Rules.

Now head on to, "**Firewall**" Tab and select "**NAT**" from  the drop down menu.

now create two port forwarding rules for WAN1 interface. 

forward OpenVPN connections to the localhost which are comming from WAN1 & WAN2 interfaces.

> For example: If there are two WANs and the OpenVPN server is running on port 1194, set the Interface to Localhost, then add two port forwards:

**WAN1 - UDP, Source *, Destination WAN1 Address port 1194, redirect target 127.0.0.1 port 1194**
**WAN2 - UDP, Source *, Destination WAN2 Address port 1194, redirect target 127.0.0.1 port 1194**

Now create the same rule for the WAN2 interface.

##### STEP 03: Install "OpenVPN Client Export" package

Move to "**System**" menu and select "**Package Manager**" 

then search  for the "**openvpn-client-export**" package

and install.

STEP 03: Create OpenVPN Users

Then move to "**System**" and then "**User Manger**" and Create an OpenVPN client user.

Create an user certificate for each user by enabling  "**Certificate**" option

##### STEP 04: OpenVPN Server Modification

Finally head on to "**Client Export**" section in the OpenVPN.

And modify "**OpenVPN Server**" settings.

Now, Change "**Host name resolution**" into "localhost"

and move down.

and add these entries to the "**Advanced Configurations**". 

These are the Public IP addresses which, OpenVPN connection going to  use.

`remote 192.168.0.2 1194 udp`

`remote 192.168.10.250 1194 udp`

OK, Now OpenVPN configuration with Dual-WAN connections is completed. Now, It work  as a fail-over connection to either connection. If one link goes down, another interface  come into  action.
Specially keep in mind, If one link goes down, it will take nearly 60 seconds to establish connection with the other interface.


##### Bottom line:

**Now, I think may you learn how to  setup fail-over Multi-WAN OpenVPN server on pfSense. Hope  this helps you to achieve this task.**
**If feel it worth, don't forget to subscribe me for more tech videos like this.**

**Thanks you & see you on next tutorial.**


[<img src="/images/Docker-Installation/sub.gif">](https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ/videos?view_as=subscriber) 

[![Foo](/images/Docker-Installation/sub.gif)](https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ/videos?view_as=subscriber)