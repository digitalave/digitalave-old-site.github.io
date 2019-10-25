---
layout: post
authors: [dimuthu_daundasekara]
title: 'Setup Postfix To Send Emails Using Gmail Relay'
image: /images/Docker-Installation/docker.jpg
tags: [Postfix, Gmail, CentOS 7, RHEL 7]
category: Spring
comments: true
---

## Setup Postfix To Send Emails Using Gmail Relay

In this tutorial I'm going to tech you how to configure an email relay server for the with your Gmail account.
Which helps you to send email through the Linux terminal and email automation tasks such as Nagios, Zabbix, and many more other scripts.

Here I'm going to set up the email server with authentication using smtp.gmail.com as a relay host in the following way

STEP 01: Generate App Password For Postfix

#### 1. First you need to have a Gmail email account.

Login to Google account from here:

Manage your account access and security settings.

Scroll down to “Password & sign-in method" and click 2-Step Verification. Then enable it.

<a href="https://myaccount.google.com/security" target="_blank">https://myaccount.google.com/security</a>

#### 2. Click the following link to Generate an App password for Postfix.

<a href="https://security.google.com/settings/security/apppasswords" target="_blank">https://security.google.com/settings/security/apppasswords</a>

#### 3. Genarate an App Passwaord

Click Select app and choose Other (provide any name) from the drop-down menu. Enter “Postfix” and click Generate . And save the password.

STEP 02: Install & Configure Postfix Mail Server

#### 1. Install Postfix Package

```bash
[root@nagios ~]# systemctl restart postfix
```


#### 2. Create SASL Password to able to authenticate with the Gmail server

Open or create the /etc/postfix/sasl/sasl_passwd file and add the SMTP Host, username, and password information:
Store Password Inside /etc/postfix/sasl_passwd

> [smtp.gmail.com]:587 <GMAIL_ID>@gmail.com:<APP PASSWORD>


```bash
[root@nagios ~]# vim /etc/postfix/sasl/sasl_passwd
```

```bash
[smtp.gmail.com]:587 username@gmail.com:exhhwavdchsihk3l
```

#### 6. Create a HASH DB for the password

Create the hash db file for Postfix by running the postmap command:

```bash
postmap /etc/postfix/sasl/sasl_passwd
```

Compile password to db (we can safely remove previous clear-text file now)

#### 7. Set required ownership/permissions for the following files:

```bash
sudo chown root:root /etc/postfix/sasl/sasl_passwd /etc/postfix/sasl/sasl_passwd.db
sudo chmod 0600 /etc/postfix/sasl/sasl_passwd /etc/postfix/sasl sasl_passwd.db
```

#### 8. Modify relayhost directive in /etc/postfix/main.cf to match the following example:

Define the following directives at the end of **/etc/postfix/main.cf** file:

```bash
# This tells Postfix to hand off all messages to Gmail, and never do direct delivery.
relayhost = [smtp.gmail.com]:587

# This tells Postfix to provide the username/password when Gmail asks for one.
# Enable SASL authentication
smtp_sasl_auth_enable = yes
# Disallow methods that allow anonymous authentication
smtp_sasl_security_options = noanonymous
# Location of sasl_passwd
smtp_sasl_password_maps = hash:/etc/postfix/sasl/sasl_passwd
# Enable STARTTLS encryption
smtp_tls_security_level = encrypt
# Location of CA certificates
smtp_tls_security_level = verify
smtp_tls_CAfile = /etc/ssl/certs/ca-bundle.crt
```


#### 9: Enable & Restart Postfix Service 
```bash
[root@nagios ~]# systemctl enable postfix
[root@nagios ~]# systemctl restart postfix
```

#### 10: Sending an Test Email:

```bash
echo  "Mail Body - Test Message from NAgiso Digital Avenue" | mailx -vvv -s "Subjct is Mail Sending from Digital Avenue" username@gmail.com
```



