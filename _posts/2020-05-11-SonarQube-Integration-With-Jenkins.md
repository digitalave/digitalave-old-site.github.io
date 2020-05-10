---
layout: post
authors: [dimuthu_daundasekara]
title: 'How To Integrate GitLab With Jenkins'
image: \images\Sonar-Jenkins\sonar-jenkins.jpg
tags: [Jenkins, GitLab, CICD, Automation,Continuous Integration, Continuous Delivery,SonarQube]
category: Spring
comments: true
---


SonarQube Integration With Jenkins

Goal ?

I want to ensure quality of the code, identify bugs, code vulnerabilities, code smells and align with code standards, after I committed codes into repositories such as Github, Gitlab. And the same way build my code automatically, using with Jenkins. I want to perform this task every time when commit code and see the static code analysis report at SonarQube.

In this case, GitLab-Jenkins-SonarQube integration comes to play.

So, In this tutorial, I'm going to demonstrate how to integrate SonarQube with Jenkins server.

Work Flow - How It Goes ?

Developer commit code changes to the GitLab/GitHub. Then, Jenkins server will fetch/pull code changes from Git repository and do a static code analysis using Sonar-Scanner and send analysis reports to SonarQube server. Finally, Jenkins build the project code.


Before You Begin !!!

I Assume...

1. You have a Pre-Configured Jenkins server

If not ? Refer this article to complete Jenkins Installation


2. You have a Pre-Configured SonarQube server

If not ? Refer this article to complete SonarQube Installation

2. You have a GitLab/GitHub account with developer role.

If not ? Refer this article to complete GitLab-Jenkins integration

3. You have integrated GitLab/GitHub with Jenkins server

STEP 01: Generate User Token

Log in to SonarQube Server and go-to "My Account" section on your profile. And move to "Security" tab. Then, Generate a "User Access Token"

Login > Profile > My Account > Security > Generate Token

IMG1

You need to copy & save this code immidiately. This code won't be able to see again. It shows only once.

IMG2

Jenkins-Auth-Token : 7a09705df7d034b99459b5127303a8315f5bdf6d

STEP 02: Install Sonar-Scanner on Jenkins

Let's move on to your Jenkins server and install following plugins.

SonarQube Scanner for Jenkins	
Plain Credentials Plugin	
Credentials Plugin

Manage Jenkins > Manage Plugins > Available [TAB] > Search For SonarQube > Install Plugins

IMG3
IMG4

Restart once plugins installed on Jenkins server.

STEP 02: Add SonarQube Authentication Token Into Jenkins

Head-over to  Jenkins server and goto Jenkins > Credentials > System > Global Credentials > Add Credentials 

Kind : Secret test

Secret : SonarQube Authentication Token

Description : Provide a descriptive name

Click OK to add new credentials.

IMG5

STEP 03: Add SonarQube Server on Jenkins

Now, We need to  add SonarQube server settings into Jenkins.

Manage Jenkins > Configure System > SonarQube servers [Scrol Down]

Add following settings in the "SonarQube server" section.

Enable :  Enable injection of SonarQube server configuration as build environment variables 	

Name : Provide a descriptive name for the connection

Server URL : Provide you SonarQube server URL with port number.

Server Authentication Token : Select credentials that we added previously as the step 02.

Apply & Save.

IMG6

STEP 04: Add Sonar-Scanner For Jenkins 

Goto Jenkins > Manage Jenkins > Global Tool Configuration > SonarQube Scanner [Scrol Down] > Add SonarQube-Scanner


Now, We have two options, Either we can install automatically or manually. If you install specific version manually, Then you need to  define "SONAR_RUNNER_HOME" path manually.

In this case, I'm going to  install automatically.

Name : Sonar Scanner 4

Install Automatically : Enabled 

Version : Select a version

IMG7

Finally Save & Apply changes.

STEP 05: Create a Jenkins Job

Create a new freestyle project and do the following configurations.

Goto Jenkins > New Item > Enter Project Name > Select FreeStyle Project > OK

IMG8

Fillout details on genaral secsion

IMG9

Here I have used GitLab for my source code management task. 
In here you can provide your own Github/GitLab repository URL and SSH Key for GitLab, as show in my "GitLab integration with Jenkins" tutorial.

IMG10

IMG11

Now, Let's move on to "Build" secsion, and click "Add build step" and select "Execute Sonar Scanner" option

Build > Add Build Step > Execute Sonar Scanner 

Task to run : Difine a name for Scanner

JDK : Leave it default or set your own JAVA 

Path to project properties : Use this, if you load analysis properties load from a sonar-prject.properties file

Analysis properties : Define Analysis Properties



project.settings=/var/lib/jenkins/tools/sonar-project.properties
sonar.projectBaseDir=/var/lib/jenkins/workspace/{YOUR_PRJECT_DIRECTORY}
sonar.language={YOUR LANGUAGE}
sonar.login={SONARQUBE_API_TOKEN}
sonar.projectVersion=1.0
sonar.sources=.
sonar.verbose=true

IMG12

Save & Apply changes you made.
Now, Rest of the cofiguration has been completed. Now, Head Over to  your project home on Jenkins and run "Build Now" button.

IMG13

Now, Goto console output will show you long list and you'll "EXECUTION SUCCESS" status. 

IMG15
IMG16

Great, Now head over to your SonarQube server. And you'll see the anaylsis report for your nely built project.

IMG17
IMG18
IMG19
IMG20
IMG21
IMG22


REF: https://www.decodingdevops.com/sonarqube-integration-with-jenkins-for-code-analysis/

https://medium.com/@amitvermaa93/jenkins-sonarqube-integration-129f5c49c4ca

https://medium.com/@anusha.sharma3010/integrating-jenkins-with-sonarqube-500578fd1770














