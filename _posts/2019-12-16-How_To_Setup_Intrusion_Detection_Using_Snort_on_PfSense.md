---
layout: post
authors: [dimuthu_daundasekara]
title: 'How To Setup Intrusion Detection Using Snort on PfSense'
image: /images/pfBlocker-pfsense/pfblocker_N.jpg
tags: [SquidGuard, Firewall, pfBlockerNG, pfsense]
category: Spring
comments: true
---

## How To Setup Intrusion Detection Using Snort on PfSense

**Snort** is an intrusion detection and prevention system.
Snort protects your network against hackers, security threats such as exploits, DDOS attacks and viruses. 
Network security is on of the Highly demand discussion now a days.

In this case you should consider deploying IDS or IPS system to detect and protect your network from attackers.

### STEP 01: Install Snort

In order to install Snort.

Headover to... 

**System > Package Manager > Available Packages**

<img src="/images/Snort-pfsense/snort_1_N.jpg" width="100%">

<img src="/images/pfBlocker-pfsense/1.png" width="100%">

<img src="/images/Snort-pfsense/snort_1_N.jpg" width="100%">

Search for a package named "**snort**"

<img src="/images/Snort-pfsense/snort_2_N.jpg" width="100%">

Hit **Install** & then **confirm** the installation to proceed. 

It grabs required repos from pfsense repositories.

<img src="/images/Snort-pfsense/snort_3_N.jpg" width="100%">

<img src="/images/Snort-pfsense/snort_4_N.jpg" width="100%">

### STEP 02: Configure Snort Global Settings

**Services > Snort > Global Settings**

To setting up Snort for the first time we need to head over to "**Global Settings**" tab and enable required rule sets to be downloaded. 

* **Snort VRT** : **Enabled**

<img src="/images/Snort-pfsense/snort_5_N.jpg" width="100%">

Either you can sing up as a free user or paid user. After you singed up,  you will get a "snort OinkMaster Code"

First of all you'll need to get registered on www.snort.org.
They will provide a unique code after the registration.

Now I'm going to sign up for a free account.

REF: <a href="https://www.snort.org/" target="_blank">https://www.snort.org/</a>

<img src="/images/Snort-pfsense/snort_6.1_N.jpg" width="100%">

<img src="/images/Snort-pfsense/snort_6.2_N.jpg" width="100%">

<img src="/images/Snort-pfsense/snort_6.3_N.jpg" width="100%">

They do have thousands of pre defined rule sets. These, rules are the source for all of these. 

Phase it here. In order to  download the rule sets.

This rule database update every 24hrs.. 
Rules are the source for Snort.

The Snort GPLv2 Community Rules and the Emerging Threats Open Rules are both available for free with no registration required. In here we can enable both of them as well.

* **Snort GPLv2 : Enabled**
* **Emergin Threats (ET) Open : Enabled**
* **OpenAppIP : Enabled**
* **RULES OpenAppID : Enabled**

**Application detection (AppID)**

OpenAppID detectors and rules, Snort package enables application detection and filtering.

OpenAppID has an ability to look at the application layer. Which is Layer 7.  Which can look at the applications which running in the system.

<img src="/images/Snort-pfsense/snort_7_N.jpg" width="100%">

##### Rules Update Settings

* **Update Interval : 1 Day**

##### General Settings

* **Remove Blocked Hours Interval : 30 Mins**

If you need to allow access again into blocked hosts automatically. If someone did some nasty thing and then the machine will be blocked and, then if again he may needs access to...

Hit "**Save**"

<img src="/images/Snort-pfsense/snort_8_N.jpg" width="100%">


### STEP 03: Update Snort Rule Databases

Now head over to Update tab and hit the "**Update Rules**" button  to download the latest rule package updates.

**System > Snort > Update Rules**

It will download all required rules automatically.

<img src="/images/Snort-pfsense/snort_9.1_N.jpg" width="100%">

<img src="/images/Snort-pfsense/snort_9.2_N.jpg" width="100%">

### STEP 04: Add Snort To an Interface

Now, let's move to "**Snort Interfaces**" tab and add new Snort interface.

Services > Snort > Interfaces

**Snort Interfaces > Add New Interface**

<img src="/images/Snort-pfsense/snort_10_N.jpg" width="100%">

* **Enable Interface**
* **Always selecet WAN Interface**
* **Provice a Description**
* **Send Alterts to System Logs**
* **Block Offenders : Enabled**
* **Search Optimize: Enable search optimization**

<img src="/images/Snort-pfsense/snort_11_N.jpg" width="100%">

And finally hit save


<img src="/images/Snort-pfsense/snort_12_N.jpg" width="100%">

### STEP 05: Select Which Types of Rules Will Protect The Network

Head over to  "**Interfaces**" and Select an configured interface, hit the **edit** button. And move to  "**WAN Categories**" tab.

**Services > Snort > Edit Interface> WAN2** 

##### Snort VRT IPS Policy Selection

* **User IPS Policy : Enabled**
* **IPS Policy Selection : Security**

<img src="/images/Snort-pfsense/snort_13.1_N.jpg" width="100%">

If you are not familiar with the Snort,  I recommend you to use "**Connectivity**" option as a starting point.

In here I'm going to use "**Security**" as my **IPS Policy**.

Select the rule-sets (Categories) Snort will load at startup:

If VRT Rule set not being used, You can select other rule set by checking the check boxes to be used.

<img src="/images/Snort-pfsense/snort_13.2_N.jpg" width="100%">

Ignore rest of other settings below.

Move down further and hit the "**Save**" button.

### STEP 06: WAN Pre-processor Configuration

Now, I'm going to configure Snort further.
Let's, head over to...

**Services > Snort > Snort interfaces > WAN Pre-Processes** 

* **Enable Performance Stats** : If you wanna have logging in depth details through the rules.

* **Auto Rule Disable : Enabled**

<img src="/images/Snort-pfsense/snort_14_N.jpg" width="100%">

Move down further and go to...

##### Application ID detection with OpenApp ID:

* **Application ID Detection: Enabled**

<img src="/images/Snort-pfsense/snort_15_N.jpg" width="100%">

Enabling OpenAppID and its rules is done from Snort Global Settings. Select both check-boxes to enable detectors and rules download. Save the page. So, double check whether both check boxes were checked.

<img src="/images/Snort-pfsense/snort_16_N.jpg" width="100%">

Better,  If rest of settings leave as it is...

Then, Hit save 

Make sure to "**Update**" Once **AppID** enabled.

### STEP 07: View Detected Apps

Now move to "**Alerts**" tab to view the detected applications.

Ok, I'll show you the alerts after snort service started.

### STEP 08: Start Snort Service

Let's head-over to

**Services > Snort > Interfaces** 

and click on the small **play** button. Then it will start the service on the WAN interface.

### STEP 09: Getting to know the alerts

**Status > System Logs**

All these will be logged under **General Logs**

Also you can use "**Alerts**" tab to view alerts generated by Snort.


**Service > Snort > Alerts**

<img src="/images/Snort-pfsense/snort_17_N.jpg" width="100%">

### STEP 10: Managing blocked hosts

Now, Move to **Service > Snort > Blocked Hosts**

The "**blocked**" tab shows that hosts are currently being blocked by Snort.

Before that, Make sure that you have enabled "**Block Offenders**" option in the selected "**interface Settings**" tab.

Also you can see what events has been blocked by Snort. 

In here you can see most of activities going on now.

<img src="/images/Snort-pfsense/snort_18_N.jpg" width="100%">

### STEP 11: Managing Pass lists
Now, Let's move to the...

**Services > Snort > Pass List**

tab.

"**Pass Lists**" are lists of IP addresses that Snort should **never blocked by Snort**.

In here you can add or define "**Firewall Aliases**" to bypass the Snort.

<img src="/images/Snort-pfsense/snort_19_N.jpg" width="100%">

**Bottom Line:**

Snort on pfSense bit resource hungry application. therefore,  You may need to upgrade your hardware resources on pfsense box.

Check the resource utilization before implemented on a production environment.

OK, Now you may have some idea and knowledge to playing around with the Snort Intrusion Detection Service. Hope this helps you to keep your network away from unwanted attackers. 

Finally, Please don't forget to Like, Comment and hit the **Subscribe** for video guides like this. See you from a new video.

REF: 

<a href="https://pfsense-docs.readthedocs.io/en/latest/ids-ips/setup-snort-package.html" target="_blank">https://pfsense-docs.readthedocs.io/en/latest/ids-ips/setup-snort-package.html</a>

<a href="https://turbofuture.com/internet/How-to-Set-Up-an-Intrusion-Detection-System-Using-Snort-on-pfSense-20" target="_blank">https://turbofuture.com/internet/How-to-Set-Up-an-Intrusion-Detection-System-Using-Snort-on-pfSense-20</a>

<a href="https://community.spiceworks.com/how_to/126207-set-up-snort-on-pfsense-for-ids-ips" target="_blank">https://community.spiceworks.com/how_to/126207-set-up-snort-on-pfsense-for-ids-ips</a>

<a href="https://linoxide.com/firewall/install-configure-snort-pfsense-firewall/" target="_blank">https://linoxide.com/firewall/install-configure-snort-pfsense-firewall/</a>

<a href="https://vorkbaard.nl/installing-snort-for-idsips-on-pfsense-2-4/" target="_blank">https://vorkbaard.nl/installing-snort-for-idsips-on-pfsense-2-4/</a>

<a href="https://www.moh10ly.com/configuring-snort-on-pfsense/" target="_blank">https://www.moh10ly.com/configuring-snort-on-pfsense/</a>

<a href="https://www.snort.org/" target="_blank">https://www.snort.org/</a>