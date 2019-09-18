---
layout: post
authors: [dimuthu_daundasekara]
title: 'Configure Local DHCP Server & DNS Resolver on pfSense'
image: /images/elk-stack/ELK.png
tags: [pfsense, DNS, DHCP, Firewall]
category: Spring
comments: true
---

### CONFIGURE DHCP SERVER & DNS RESOLVER ON PFSENSE

Welcome to digital avenue. Hi Guys today i will goning to demonstrate how to install anbd configure dhcp server and dns reslover on pfsense 2.4.

#### STEP 01: GENARAL CONFIGURATION

**Systemc > Genaral Setup**

Goto "**System**" tab and select "**Genaral Setup**" from the drop down menu.

![](/images/pfsense-dns-dhcp/screenshot_01.png)

Change following fields as seen below.

**Hostname** : Define a meaningfull hostname for pfSense.

**Domain Name** : Define your domain name which pfsense router used.

**DNS Server** : Define public authoritative DNS servers for user pfSense itself.

![](/images/pfsense-dns-dhcp/screenshot_02.png)

#### STEP 02: SETUP DHCP SERVER

Goto  Services tab and select DHCP Server from  the drop down menu.

**Services > DHCP Server**

![](/images/pfsense-dns-dhcp/screenshot_2.png)

Select the interface which you want to DHCP server runing on.

Fill the relevent fields with "Subnet", "Subnet Mask", "Range", "DNS Servers", "Gateway" and "Domain Name"

![](/images/pfsense-dns-dhcp/screenshot_3.png)

![](/images/pfsense-dns-dhcp/screenshot_4.png)

![](/images/pfsense-dns-dhcp/screenshot_5.png)

Change all other options according to your requirement.

#### STEP 02: SETUP DNS SERVER

Unbound is integrated into pfSense. Unbound is use as the DNS server. 

Go to  "Services" tab and select "DNS Resolver" from the drop down menu.

**Services > DNS Resolver**

![](/images/pfsense-dns-dhcp/screenshot_6.png)

**Enable DNS Resolver:** Enable/Disable DNS Resolver

**Network Interfaces:** Network interfaces which are listening from  DNS queries from  clients.

![](/images/pfsense-dns-dhcp/screenshot_7.png)

**Outgoing Network Interfaces:** Specific interfaces which are passing outbound DNS queries to higer level DNS server such as Google DNS. Most cases WAN interfaces used.

**Enable DNSSEC Support:** DNSSEC provides added security to DNS queries which validate DNS queries.

**Enable Forwarding Mode:** Unbound DNS queries forwarding to upstream DNS server which are defined under  System > General 

**Register DHCP leases in the DNS Resolver:** DHCP static mappings can be registered in Unbound which enables the resolving of hostnames that have been assigned addresses by the DHCP server in pfSense

![](/images/pfsense-dns-dhcp/screenshot_8.png)

**Host Overrides:** Allows creation of custom DNS responses/records to create new entries that do not exist in DNS outside the firewall, or to override DNS responses for other hosts.

![](/images/pfsense-dns-dhcp/screenshot_9.png)

**Domain Override:** For domains that should be queried by a specific remote server.
At this point I want to redirect all DNS quest for  .youtube.com and .facebook.com redirect to localhost/127.0.0.1 itself.

![](/images/pfsense-dns-dhcp/screenshot_10.png)

![](/images/pfsense-dns-dhcp/screenshot_11.png)

#### STEP 02: Verifing DNS Queries.

![](/images/pfsense-dns-dhcp/screenshot_11.png)