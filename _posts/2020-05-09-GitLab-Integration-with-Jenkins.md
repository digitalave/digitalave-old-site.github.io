---
layout: post
authors: [dimuthu_daundasekara]
title: 'How To Integrate GitLab With Jenkins'
image: \images\SonarQube-Ubuntu\gitlab-jenkins.jpg
tags: [Jenkins, GitLab, CICD, Automation,Continuous Integration, Continuous Delivery,SonarQube]
category: Spring
comments: true
---


## How To Integrate GitLab With Jenkins

**Before You Begin !**

In this tutorial, I assume you have already configured a Jenkins server and having GitLab account with you. If you don't have a Jenkins server, Please refer this article to configure a Jenkins server.

<a href="https://digitalave.github.io/spring/2020/04/06/How-To-Install-and-configure-Jenkins-on-Ubuntu-18.04.html" target="_blank">How To Install & Configure Jenkins</a> : <a href="https://digitalave.github.io/spring/2020/04/06/How-To-Install-and-configure-Jenkins-on-Ubuntu-18.04.html" target="_blank">https://digitalave.github.io/spring/2020/04/06/How-To-Install-and-configure-Jenkins-on-Ubuntu-18.04.html</a>


If you don't have GitLab account, Please create an account.

**REF**: <a href="https://gitlab.com/" target="_blank">https://gitlab.com/</a>

### Prerequisites : 

* Pre-Configured Jenkins Server 

* GitLab Account + Developer Role Permission


### Introduction : 

**Jenkins** is an open source software development platform enriched with continuous integration (CI) and many more DevOps automation capabilities.

Organizations using Jenkins to build and deploy their applications and integrate with GitLab for all other capabilities which has.

**GitLab - Jenkins integration allows you to build and deploy application on Jenkins and reflect the output on  the GitLab UI more convenience**

GitLab & Jenkins integration allows you to trigger a Jenkins build when a code is pushed to a repository, or when a merge request is created. 

### STEP 01: Install GitLAB Plugins on Jekins Server

Go to Manage Plugin section, then search and install following plugins on your Jenkins server.

`"Git"`

`"GitLab Plugin"` 

`"GitLab API Plugin"`

`"Credentials Plugin"` 

`"GitLab Authentication Plugin"`


**Manage Jenkins > Manage Plugins > Available Plugin [Search] > Install & Restart** 

<img src="\images\GitLab_Jenkins\1.png" width="auto" width="50%">
<img src="\images\GitLab_Jenkins\2.png" width="auto" width="100%">
<img src="\images\GitLab_Jenkins\3.png" width="auto" width="100%">


### STEP 02: Create a New GitLab User / Promote Existing GitLab User 

Create a new GitLAB user with "**Developer**" role permission and grant access to each repository/project you want to integrate with Jenkins

**Note: User should have Developer Permission Role**

### STEP 02: Generate Personal Access Token

Now, Let's head-over to GitLAB account and move to your profile setting section.

**User Settings > Access Tokens** 

<img src="\images\GitLab_Jenkins\4.png" width="auto" width="100%">

Create a new Personal Access Token for Jenkins authentication.

**Name : Provide Token Name**

**Scope : API** - Which Grant access to GitLab resources such a Projects, Groups and Registries.

<img src="\images\GitLab_Jenkins\5.png" width="100%">
<img src="\images\GitLab_Jenkins\6.png" width="100%">

Save the deployed token somewhere safe. Once you leave or refresh the page, you won’t be able to access it again.

**NOTE: Once you've generated a Personal Access Token copy it and save in a separate file immediately.Because, Token only visible only once.**


`Jenkins-GitLAB_API-Access : i5Jfi8a5foEFoC9CkVzl`

### STEP 03: Add GitLab Credentials to Jenkins

Again, Head-over to Jenkins server and Then, We need to add an authentication token into Jenkins server. 

**Jenkins > Credentials > System > Global Credentials > Add Credentials**

<img src="\images\GitLab_Jenkins\7.png" width="100%">

Scroll down little and click on "**Global Credentials**" under Domain column and "**Add Credentials**"

<img src="\images\GitLab_Jenkins\8.png" width="100%">
<img src="\images\GitLab_Jenkins\9.png" width="100%">
<img src="\images\GitLab_Jenkins\10.png" width="100%">

Select following options for the "**Global Credentials**" section.

**Kind : GitLab API Token**

**API Token : Add Previously Generated Personal Access Token**

**Description : Provide a Descriptive Name** 

### STEP 03: Configure GitLab Settings on Jenkins Server

Let's move on to "GitLab" configuration section in **Manage Jenkins > Configure System**.

**Manage Jenkins > Configure System > "GitLAB" Configuration Section.** 

Then, Configure following entries...

**Enable authentication for '/project' end-point : Enable**

**Connection Name : Provide a Descriptive Name**

**GitLab host URL : GitLab server URL**

**Credentials : Select previously added credentials from the drop-down menu.**

<img src="\images\GitLab_Jenkins\11.png" width="100%">

Finally, Check whether connection is successful by pressing "**Test Connection**" button.
If connection successful. Then move to next step.


Now, Connection between Jenkins and GitLab is OK. GitLab API plugin used to access for Jenkins to get metadata from  GitLab.

But, We need to have a SSH key authentication to commit changes from Jenkins to GitLab.

### STEP 04: Configure Jenkins Project

Let's create a **Public-Private SSH Key Pair** add them into GitLab and Jenkins respectively.

Create a new project on GitLab or use existing project on GitLab

In this case, I'm going to use my existing project. Now, we need to create ssh key pair, that use for getting access for our GitLab private repositories.

#### A. Create SSH Key Pair on Jenkins

Login to Jenkins host terminal console and switch  to "**jenkins**" user and move to Jenkins home directory. and generate a SSH-Key-Pair.

```bash
root@SRV3:~# su - jenkins
jenkins@SRV3:~$ cd /var/lib/jenkins/
jenkins@SRV3:~$ ssh-keygen 
Generating public/private rsa key pair.
jenkins@SRV3:~$ cd /var/lib/jenkins/.ssh/
jenkins@SRV3:~/.ssh$ ls
id_rsa	id_rsa.pub  known_hosts
```


Open "id_rsa.pub" file and copy content.


Go to GitLAB and deploy a new ssh key. Go to your GitLab profile "**Setting**" and then go to "**SSH Keys**" section.

Paste code that we copied content from "id_rsa.pub" file.

<img src="\images\GitLab_Jenkins\13.png" width="100%">

<img src="\images\GitLab_Jenkins\14.png" width="100%">

Now, Public key has been added on GitLab server.

Then, We need to add our private key into Jenkins server.

Open "/var/lib/jenkins/.ssh/id_rsa" file and copy content from private key. 

```bash
vim /var/lib/jenkins/.ssh/id_rsa
```


Go to **Jenkins > Credentials > System > Global Credentials > Add Credentials** 

Click the "Add" button next to the "Credentials" field and select the "Jenkins" option.
In the resulting dialog, select "SSH Username with private key" as the credential type, set the "Username" to git, and enter the content of the private key selected for use between GitLab and Jenkins.

**Kind : SSH Username with private key**

**Description : Provide descriptive name**

**Username : git**

**Private Key : Enter private key** 


And also remember that you already attached the corresponding public key to your GitLab profile in Step in previous step.

Add copied public key content into "Key" section.

<img src="\images\GitLab_Jenkins\17.png" width="100%">

### C. Create FreeStyle Project on Jenkins

Now, It's time to create a new project and do further configuration.

**Jenkins > New Item > FreeStyle Project** 

Give a name to the project and continue.

<img src="\images\GitLab_Jenkins\15.png" width="100%">

<img src="\images\GitLab_Jenkins\16.png" width="100%">

Now, Head-over to  "**source code management**" section and select "**Git**"

**Repository URL : git@gitlab.com:dimuit86/shadowsocksx-ng-develop.git**

**Credentials : Select added credentials from drop down menu**

Attach new credentials to the SSH URL for the GitLab repository.

<img src="\images\GitLab_Jenkins\18.png" width="100%">

On the same configuration page, find the "**Build Triggers**" section and check the option to "**Build when a change is pushed to GitLab**".

<img src="\images\GitLab_Jenkins\19.png" width="100%">

Finally Apply & Save Changes 

Now, GitLab integration with Jenkins has been completed. Now, We can check the connectivity, using build button and checking console logs.

<img src="\images\GitLab_Jenkins\20.png" width="100%">

<img src="\images\GitLab_Jenkins\21.png" width="100%">

Voilà, It's working. 

