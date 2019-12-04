---
layout: post
authors: [dimuthu_daundasekara]
title: 'Internet Content Filtering / Site Blocking Using pfBlockerNG on pfSense'
image: /images/pfBlocker-pfsense/pfblocker.jpg
tags: [SquidGuard, Firewall, pfBlockerNG, pfsense]
category: Spring
comments: true
---

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/embed/Dqe7W_mtrH0?&autoplay=1' frameborder='0' allowfullscreen></iframe></div>

### Internet Content Filtering / Site Blocking Using pfBlockerNG on pfSense

pfBlockerNG extent the capability of the pfsense firewall beyond the traditional state full firewall.

pfBlockerNG provides ability to pfsense firewall to make allow/deny decisions based upon items such as Geo-location, IP address, Alexa rating and the domain name.

In this guide,  I will  walk through the configuration of pfBlockerNG on pfsense as a domain and content filter.

### Web Site Filtering using pfBlocker using pfSense:

#### Filtering Content with DNS:

SquidGuard doesn't work properly with mobile phone and IOT devices.
Because, Squid can't do the man in the middle. most sites moved into SSL encryption. because most of site using encrypted SSL encryption. That's why Squid can't do the man in the  middle and filter some websites.

### Requirements: 

**pfSense should be the default DNS server which pointed into client's hosts.**


<img src="/images/pfBlocker-pfsense/1.png" width="100%">

* Create Two Firewall Rules For DNS 

<img src="/images/pfBlocker-pfsense/2.png" width="100%">


### STEP 01: Install pfBlockerNG Package

First of all, you need to install the package on pfSense appliance.
Once logged in to the main pfSense page, click on the "System" drop down and then select "Package Manager".

Now head over to System > Package Manager > Available Packages

<img src="/images/pfBlocker-pfsense/3.png" width="100%">

Once the ‘Available Packages’ page loads, type ‘pfblocker’ into the ‘Search term’ box and click the ‘Search’. The first item that is returned should be pfBlockerNG. Locate the ‘Install’ button to the right of the pfBlockerNG description and click the ‘+’ to install the package.

<img src="/images/pfBlocker-pfsense/4.png" width="100%">

The page will reload, then continue the installation by clicking "Confirm".

In Search section, fill the following fields:

Search terms: Type pfBlockerNG

Click on Search button

In Packages section, the pfBlockerNG will be appear

Click on + Install and then on Confirm buttons to launch installation

<img src="/images/pfBlocker-pfsense/5.png" width="100%">

Once installation is completed, pfBlockerNG appears in System > Package Manager > Installed Packages

Now, pfBlockerNG configuration can begin.
Before that we need to reconfigure our pfsense DNS resolver according to our requirement which need to redirect DNS requests in order to filter out bad domains. This means clients on the LAN interface need to use the pfSense as the default and primary DNS resolver.

### STEP 02: Configure DNS Resolver

Now, Head over to  "Services" and select "DNS Resolver" from  the drop down menu.

<img src="/images/DNS-pfsense/1.png" width="100%">

<img src="/images/DNS-pfsense/2.png" width="100%">

* Enable DNS Resolver

* Set the listning port 53

* Ingress Network interface should be LAN & localhost

* Egress Outgoing Network Interface should be WAN interfaces

Finally hit "Save" & "Apply Changes" 

### STEP 03: Configure pfBlockerNG - General Settings

Next step  is the configuration of pfBlocker specifically.

Head over to  general tab do the following changes.

Let's move to Firewall > pfBlockerNG > General

<img src="/images/pfBlocker-pfsense/6.png" width="100%">

In General Settings section, fill the following fields:

<img src="/images/pfBlocker-pfsense/7.png" width="100%">

* Enable pfBlockerNG: Checked
* Keep Settings: Checked
* Cron Settings: Select Every hour, select 0 as minute, hour and Daily/Weekly

Move down bit further...

In Interface/Rules Configuration section, fill the following fields:

* Inbound Firewall Rules: Select WAN interfaces and Block
* Outbound Firewall Rules: Select LAN interfaces and Reject

<img src="/images/pfBlocker-pfsense/8.png" width="100%">

Rest of the other settings leave as it is...

and click save.

### STEP 04: Configure DNSBL 

Now move t0 DNSBL tab and do the following changes.

To configure DNSBL, go to Firewall > pfBlockerNG > DNSBL > DNSBL

<img src="/images/pfBlocker-pfsense/9.png" width="100%">
<img src="/images/pfBlocker-pfsense/10.png" width="100%">


In DNSBL section, fill the following fields:

* Enable DNSBL: Checked
* Enable TLD: Not checked
* DNSBL Virtual IP: Enter an IP address is not in your internal networks, something like 10.10.10.10
* DNSBL Listening Port: Enter 8081
* DNSBL SSL Listening Port: Enter 8443
* DNSBL Listening Interface: Select LAN or another internal interface to listen on
* DNSBL Firewall Rule: Checked if you have multiple LAN interfaces

<img src="/images/pfBlocker-pfsense/11.png" width="100%">

In DNSBL IP Firewall Rule Settings section, fill the following fields:

* List Action: Select Deny Both
* Enable Logging: Select Enable

* In Advanced Inbound Firewall Rule Settings : Don't change 
* In Advanced Outbound Firewall Rule Settings : Don't change
* In Alexa Whitelist : Don't change

* In Custom Domain Whitelist :

<img src="/images/pfBlocker-pfsense/12.png" width="100%">

To begin, enter the following white-list domains and modify as you like.

```dns
.play.google.com
.drive.google.com
.accounts.google.com
.www.google.com
.github.com
.outlook.live.com
.edge-live.outlook.office.com # CNAME for (outlook.live.com)
.outlook.ha-live.office365.com # CNAME for (outlook.live.com)
.outlook.ha.office365.com # CNAME for (outlook.live.com)
.outlook.ms-acdc.office.com # CNAME for (outlook.live.com)
.amazonaws.com
.login.live.com
.login.msa.akadns6.net # CNAME for (login.live.com)
.ipv4.login.msa.akadns6.net # CNAME for (login.live.com)
.mail.google.com
.googlemail.l.google.com # CNAME for (mail.google.com)
.pbs.twimg.com
.wildcard.twimg.com # CNAME for (pbs.twimg.com)
.sites.google.com
.www3.l.google.com # CNAME for (sites.google.com)
.docs.google.com
.mobile.free.fr
.plus.google.com
.samsungcloudsolution.net
.samsungelectronics.com
.icloud.com
.microsoft.com
.windows.com
.skype.com
.googleusercontent.com
```

* In TLD Exclusion List : Don't change 
* In TLD Blacklist : Don't change 
* In TLD Whitelist : Don't change

Click on the Save button once all field are filling

### STEP 05: Configure DNSBL Feeds 

To configure DNSBL feeds, go to Firewall > pfBlockerNG > DNSBL > DNSBL Feeds

Click on + Add button

Go to  this URL and add preferred lists.

<a href="https://github.com/StevenBlack/hosts" target="_blank">https://github.com/StevenBlack/hosts</a>

**In DNSBL Feeds section, fill the following fields:**

* DNS GROUP Name: Enter DNSBlockListGroup
* Description: Enter DNS Block list
* DNSBL Settings – These are the actual lists
* State – Whether that source is used or not and how it is obtained
* Source – The link/source of the DNS Black List
* Header/Label – User choice; no special characters
* List Action – Set to Unbound
* Update Frequency – How often the list should be updated

<img src="/images/pfBlocker-pfsense/13.png" width="100%">


<img src="/images/pfBlocker-pfsense/14.png" width="100%">

You also can define custom black list domains.
Now, save.

<img src="/images/pfBlocker-pfsense/15.png" width="100%">

Next, Head over to the "Update" tab. 

Select "update" tick box and and hit the run button.

Now, It will  starts to update blacklist index.

<img src="/images/pfBlocker-pfsense/16.png" width="100%">

Now, Head over to "Status" tab and select "Services" from the drop  down menu.

<img src="/images/pfBlocker-pfsense/17.png" width="100%">

Now, restart both dnsbl & unbound services.

### STEP 05: Test Effectiveness by Browsing on Client Side.

<img src="/images/pfBlocker-pfsense/18.png" width="100%">

<img src="/images/pfBlocker-pfsense/19.png" width="100%">

**Reference : ** 
=================================================================
**Here are the required resources to complete this tutorial:**

My Website:
1. https://digitalave.github.io/spring/2019/12/02/site_domain_filtering_using-pfblocker_on_pfsense.html

Steven's Block List:

2. https://github.com/StevenBlack/hosts

YouTube Black List:

3.https://raw.githubusercontent.com/digitalave/digitalave.github.io/master/Config/youtube

4. Pi-Hole Black List:

https://github.com/pi-hole/pi-hole/wiki/Customising-sources-for-ad-lists

================================================================




