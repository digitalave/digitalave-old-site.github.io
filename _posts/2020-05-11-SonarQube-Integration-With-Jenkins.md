---
layout: post
authors: [dimuthu_daundasekara]
title: 'How To Integrate SonarQube With Jenkins'
image: \images\Sonar-Jenkins\sonar-jenkins.jpg
tags: [Jenkins, GitLab, CICD, Automation,Continuous Integration, Continuous Delivery,SonarQube]
category: Spring
comments: true
---


## SonarQube Integration With Jenkins

##### Goal ?

**I want to ensure quality of the code, identify bugs, code vulnerabilities, code smells and align with code standards, after I committed codes into repositories such as Github, Gitlab. And the same way build my code automatically, using with Jenkins. I want to perform this task every time when commit code and see the static code analysis report at SonarQube.**

In this case, GitLab-Jenkins-SonarQube integration comes to play.

So, In this tutorial, I'm going to demonstrate how to integrate SonarQube with Jenkins server.

##### Work Flow - How It Goes ?

Developer commit code changes to the GitLab/GitHub. Then, Jenkins server will fetch/pull code changes from Git repository and do a static code analysis using Sonar-Scanner and send analysis reports to SonarQube server. Finally, Jenkins build the project code.


##### Before You Begin !!!

**I Assume...**

1. You have a Pre-Configured Jenkins server
If not ? Refer this article to complete Jenkins Installation

2. You have a Pre-Configured SonarQube server
If not ? Refer this article to complete SonarQube Installation
3. You have a GitLab/GitHub account with developer role.
If not ? Refer this article to complete GitLab-Jenkins integration
4. You have integrated GitLab/GitHub with Jenkins server

### STEP 01: Generate User Token

Log in to SonarQube Server and go-to "**My Account**" section on your profile. And move to "**Security**" tab. Then, Generate a "**User Access Token**"

**Login > Profile > My Account > Security > Generate Token**

<img src="\images\Sonar-Jenkins\1.png" width="auto" width="100%">

You need to copy & save this code immediately. This code won't be able to see again. It shows only once.

<img src="\images\Sonar-Jenkins\2.png" width="auto" width="100%">

`Jenkins-Auth-Token : 7a09705df7d034b99459b5127303a8315f5bdf6d`


### STEP 02: Install Sonar-Scanner on Jenkins

Let's move on to your Jenkins server and install following plugins.

`SonarQube Scanner for Jenkins`

`Plain Credentials Plugin`

`Credentials Plugin`


**Manage Jenkins > Manage Plugins > Available [TAB] > Search For SonarQube > Install Plugins**

<img src="\images\Sonar-Jenkins\3.png" width="auto" width="100%">
<img src="\images\Sonar-Jenkins\4.png" width="auto" width="100%">

Restart once plugins installed on Jenkins server.

### STEP 03: Add SonarQube Authentication Token Into Jenkins

Head-over to  Jenkins server and go-to **Jenkins > Credentials > System > Global Credentials > Add Credentials** 

Kind : Secret test

Secret : SonarQube Authentication Token

Description : Provide a descriptive name

Click OK to add new credentials.

<img src="\images\Sonar-Jenkins\5.png" width="auto" width="100%">

### STEP 04: Add SonarQube Server on Jenkins

Now, We need to  add SonarQube server settings into Jenkins.

**Manage Jenkins > Configure System > SonarQube servers [Scrol Down]**

Add following settings in the "SonarQube server" section.

Enable :  Enable injection of SonarQube server configuration as build environment variables 	

Name : Provide a descriptive name for the connection

Server URL : Provide you SonarQube server URL with port number.

Server Authentication Token : Select credentials that we added previously as the step 02.

Apply & Save.

<img src="\images\Sonar-Jenkins\6.png" width="auto" width="100%">

### STEP 05: Add Sonar-Scanner For Jenkins 

**Goto Jenkins > Manage Jenkins > Global Tool Configuration > SonarQube Scanner [Scrol Down] > Add SonarQube-Scanner**


Now, We have two options, Either we can install automatically or manually. If you install specific version manually, Then you need to  define "**SONAR_RUNNER_HOME**" path manually.

In this case, I'm going to  install automatically.

Name : Sonar Scanner 4

Install Automatically : Enabled 

Version : Select a version

<img src="\images\Sonar-Jenkins\7.png" width="auto" width="100%">

Finally Save & Apply changes.

### STEP 05: Create a Jenkins Job

Create a new freestyle project and do the following configurations.

Go-to **Jenkins > New Item > Enter Project Name > Select Freestyle Project > OK**

<img src="\images\Sonar-Jenkins\8.png" width="auto" width="100%">

Fill-out details on general section

<img src="\images\Sonar-Jenkins\9.png" width="auto" width="100%">

Here I have used GitLab for my source code management task. 
In here you can provide your own Github/GitLab repository URL and SSH Key for GitLab, as show in my "GitLab integration with Jenkins" tutorial.

Refer this article to know how to use SSH key authention with Gitlab.

REF: <a href="https://digitalave.github.io/spring/2020/05/09/GitLab-Integration-with-Jenkins.html" target="_blank">https://digitalave.github.io/spring/2020/05/09/GitLab-Integration-with-Jenkins.html</a>

<img src="\images\Sonar-Jenkins\10.png" width="auto" width="100%">

<img src="\images\Sonar-Jenkins\11.png" width="auto" width="100%">


Alternatively, You also can directly enter your GitLab username and password in Jenkins > Credentials > System > Global Credentials > Add Credentials > Select "Username Password" from  the drop down menu as the option for "Kind"


Now, Let's move on to "**Build**" section, and click "**Add build step**" and select "**Execute Sonar Scanner**" option

**Build > Add Build Step > Execute Sonar Scanner** 

Task to run : Define a name for Scanner

JDK : Leave it default or set your own JAVA 

Path to project properties : In here, you can define sonar-project.properties file location. [Optional]

Analysis properties : Define Analysis Properties

```bash
sonar.projectBaseDir=/var/lib/jenkins/workspace/{YOUR_PRJECT_DIRECTORY}
sonar.language={YOUR LANGUAGE}
sonar.login={SONARQUBE_API_TOKEN}
sonar.projectVersion=1.0
sonar.sources=.
sonar.verbose=true
sonar.projectKey={PROJECT_NAME}
sonar.host.url={SONARQUBE_URL:PORT/DNS_NAME}
sonar.projectName={PROJECT_NAME}
sonar.sourceEncoding=UTF-8
sonar.project.settings=/var/lib/jenkins/workspace/{PROJECT_NAME}/sonar-project.properties
sonar.analysis.mode=publish
sonar.buildbreaker.skip=true
```

<img src="\images\Sonar-Jenkins\12.png" width="auto" width="100%">

Save & Apply changes you made.
Now, Rest of the configuration has been completed. Now, Head Over to  your project home on Jenkins and run "**Build Now**" button.

<img src="\images\Sonar-Jenkins\13.png" width="auto" width="100%">

Now, Go to console output will show you long list and you'll "**EXECUTION SUCCESS**" status. 

<img src="\images\Sonar-Jenkins\15.png" width="auto" width="100%">
<img src="\images\Sonar-Jenkins\16.png" width="auto" width="100%">

Great, Now head over to your SonarQube server. And you'll see the analysis report for your newly built project.
<img src="\images\Sonar-Jenkins\17.png" width="auto" width="100%">
<img src="\images\Sonar-Jenkins\18.png" width="auto" width="100%">
<img src="\images\Sonar-Jenkins\19.png" width="auto" width="100%">
<img src="\images\Sonar-Jenkins\20.png" width="auto" width="100%">
<img src="\images\Sonar-Jenkins\21.png" width="auto" width="100%">
<img src="\images\Sonar-Jenkins\22.png" width="auto" width="100%">


Troubleshooting Tips: 

Issue : Unable To Load Component Class --- Project.lock
		Report Task
		GitLab Errors
Resolution : Remove GitLab Plugin from SonarQube Server

https://github.com/adnovum/sonar-build-breaker
https://github.com/gabrie-allaigre/sonar-gitlab-plugin









