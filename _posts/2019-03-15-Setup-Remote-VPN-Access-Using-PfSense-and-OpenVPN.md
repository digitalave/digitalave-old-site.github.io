---
layout: post
authors: [dimuthu_daundasekara]
title: 'Setup Remote VPN Access Using PfSense and OpenVPN'
image: /images/pf-vpn/pf-openvpn.jpg
tags: [pfSense, Firewall, OpenVPN, VPN]
category: Spring
comments: true
---

# Setup Remote VPN Access Using PfSense and OpenVPN

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/embed/hqjE4KySvWU' frameborder='0' allowfullscreen></iframe></div>

Pfsense is a great firewall solution. Very reliable and comes with built in VLAN and VPN support. In this tutorial I’m going to demonstrate how to setup a user authenticated OpenVPN server in PfSense.
In this guide I assume you already have a functional pfSense firewall running.

## STEP 1: - Open OpenVPN Wizard

A. Create a Virtual Machine for Pfsense

<img src="/images/pf-vpn/image001.jpg" width="100%">

<img src="/images/pf-vpn/image002.jpg" width="100%">

Select OpenVPN Authentication Backed Type

<img src="/images/pf-vpn/image003.jpg" width="100%">

In this tutorial I have used “Local User Access” as the authenticated backed type.

## STEP 2:- Create New CA

Create a Certificate Authority to generate certificates for the OpenVPN server.

Fill out the following fields to create a new CA.

<img src="/images/pf-vpn/image004.jpg" width="100%">

## STEP 3:- Create Server Certificate

Create a Server Certificate from the CA for OpenVPN.

<img src="/images/pf-vpn/image005.jpg" width="100%">

## STEP 4:- OpenVPN Genaral Settings Configuration

In this case OpenVPN interface will listen on external facing WAN interface which is connected to the internet.

Interface: WAN

Protocol: UDP on IPv4 Only

LocalPort: 1194

Description: VPN

<img src="/images/pf-vpn/image006.jpg" width="100%">

Cryptographic Settings Configuration

This section can be left default or change it upon your security needs.

<img src="/images/pf-vpn/image007.png" width="100%">

## STEP 5:- OpenVPN Tunnel Configuration

There are two important sections.

Tunnel Network

The tunnel networl should be a new network that does not currently exist on the network or the Pfsense firewall routing table.

When client connect to the VPN they will receive an address in this network.

Ex: 172.25.0.10/24

Local Network

Enter the network address of that client will connect to local network. Network address that Pfsense box resides.

Rest of the settings can be change according to your requirement.

<img src="/images/pf-vpn/image008.png" width="100%">

## STEP 6:- OpenVPN Client Settings

The settings in the client settings section will be assigned to OpenVPN clients when they connect to the network.

If you are also using pfSense as your local DNS server, you would enter them here. Separate DNS servers also can enter here.

Optionally DNS, NTP server can be provided to the VPN clients from here.

<img src="/images/pf-vpn/image009.png" width="100%">

<img src="/images/pf-vpn/image010.png" width="100%">

## STEP 7:- Firewall Rule creation for OpnVPN

Traffic from client to server: - If this section enabled, OpenVPN wizard will automatically generate the necessary firewall rules to permit the incoming connection to Pfsense OpenVPN server from clients anywhere on the internet.

Traffic from clients through VPN:- If this connection enabled, OpenVPN wizard will automatically generate firewall rules which allow traffic from clients connected to the VPN to anywhere on the local network.

<img src="/images/pf-vpn/image011.png" width="100%">

Finally finish the wizard.

<img src="/images/pf-vpn/image012.png" width="100%">

## STEP 8:- Create VPN Users with Certificates

If you selected the “local user access” option during the VPN wizard then users can be added through the pfSense user manger.

<img src="/images/pf-vpn/image013.jpg" width="100%">

Create new user.

<img src="/images/pf-vpn/image014.png" width="100%">

<img src="/images/pf-vpn/image015.png" width="100%">

<img src="/images/pf-vpn/image016.png" width="100%">

## STEP 9:- Install OpnVPN Client Export Package

Install OpenVPN Client Export package using Pfsense package manager.

<img src="/images/pf-vpn/image017.jpg" width="100%">

<img src="/images/pf-vpn/image018.png" width="100%">

<img src="/images/pf-vpn/image019.jpg" width="100%">

After the installation there will be a new tab named with “Client Export” in OpenVPN menu.

<img src="/images/pf-vpn/image020.png" width="100%">

Modify “Hostname Resolution” field. By default this is set to the IP address of the interface running OpenVPN.

<img src="/images/pf-vpn/image021.png" width="100%">

<img src="/images/pf-vpn/image022.png" width="100%">

After any changes made, click the “Save as default” button to store the settings.

## STEP 10:- Download the OpenVPN Client Packages.

<img src="/images/pf-vpn/image023.png" width="100%">

Download and install OpenVPN client application.

> https://openvpn.net/index.php/open-source/downloads.html
> 
> https://swupdate.openvpn.org/community/releases/openvpn-install-2.4.6-I602.exe


Install downloaded OpenVPN profile.

<img src="/images/pf-vpn/image024.jpg" width="100%">

<img src="/images/pf-vpn/image025.jpg" width="100%">

<img src="/images/pf-vpn/image026.jpg" width="100%">

**Little Request:**

> I appreciate you guys taking the time in reading my post. Please check out my YouTube channel and please subscribe for more as it'll help me loads.


<a href="https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ/featured?view_as=subscriber" target="_blank">https://www.youtube.com/channel/UCovlVsoRVItner26ZJPBjmQ/featured?view_as=subscriber</a>