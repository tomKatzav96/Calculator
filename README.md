# Jenkins pipeline for Java application

Jenkins pipeline using SonarQube, Maven, and Artifactory for a basic calculator application written in Java, forked from [HouariZegai/Calculator](https://github.com/HouariZegai/Calculator).  

**Important note:** This project is based on having a Jenkins master and agent connected.

![Image](jenkins-for-java.png "Architecture of the project")  


![Image](seccess-pipline.png "Seccess pipeline")


## Create SonarQube servers
### Installing an instance of SonarQube with Docker image

Launch an instance in AWS using Ubuntu 22.04 image, instance type t2.medium, and add external port: 9000   
**Important note:** Before you continue make sure you have docker installed in this instance

1. Start the Docker container by running:
```
docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest
```
2. Update the restart policy to always
```
 docker update --restart always sonarqube
 ```

### Access the SonarQube Platform

```URL
http://<hostname>:8082
```
For Example http://localhost:8082 or http://192.168.86.243:8082
Once the platform is up, log in using username `admin` and password `admin`

### Create an authentication token in SonarQube

In the SonarQube server go to `Administration -> security -> users`  
under Token click on `Token update`   
token name: `Jenkins` (you can call it whatever you want)   
click `generate` and save the token in a note, you will use it later

### Install SonarQube Scanner for Jenkins plugin

In the Jenkins server go to `Manage Jenkins -> Manage Plugins -> available` and type `Sonar`  
the plugin that we want is `SonarQube Scanner`  
click on the check box and then click on `download now and install after restart`  
restart Jenkins and make sure that the SonarQube scanner for Jenkins is installed

### Configure a SonarQube server in Jenkins

Go to `Manage Jenkins -> Configure System` and scroll down to the `SonarQube` section  
check the `Environment variable` checkbox  
click on `Add SonarQube` to add the SonarQube server, I named it `sq1` (you can call it any name that you want)  
the server URL is the SonarQube serveer URL ( `http://<hostname>:9000` )   
add server authentication token by clicking on `add -> Jenkins`  
the kind is secret text, the secret is the token that we generate earlier, the ID and Description are `jenkins-sonar`  
click `Add`, select it, and save

## Create Artifactory servers
### Artifactory quick setup

Launch an instance in AWS using Ubuntu 20.04 image, instance type t3.medium, and add external ports: 8081, 8082.
1. Download the installer
```bash
wget -O artifactory-pro.deb "https://releases.jfrog.io/artifactory/artifactory-pro-debs/pool/jfrog-artifactory-pro/jfrog-artifactory-pro-[RELEASE].deb"
```
2. Install Artifactory
```bash
sudo apt install ./artifactory-pro.deb -y
```
3. Start the service
```bash
sudo systemctl start artifactory.service
```

### Access the JFrog Platform

```URL
http://<hostname>:8082
```
For Example http://localhost:8082 or http://192.168.86.243:8082
The JFrog Platform will take about a minute to start up
Once the platform is up, log in using username `admin` and password `password`

### Create a local repository in Artifactory

Click on `Welcome, admin` -> `New Local Repository`  
The name is `java-calculator` and the type is General.  
write the Repository Key as java-calculator and click on `create local repository`

###  Create an access token in Artifactory

Click on ` Administration -> User Management -> Access Tokens`  
Click on `Generate  Token`, Description is `Jenkins`, and `Generate`  
Copy and save the Token in your note.  

###  Create a secret text credential in Jenkins

Go to `Manage Jenkins -> Manage Credentials` and create new secret text.  
the secret is the token that you generated in the previous step.  
the Description and the ID are `artifactory-access-token` and click OK

## Create Discord Notification

First, set up Plugin Discord on Jenkins. Its name: Discord Notifier  
Second, Open Discord (Admin), on Setting -> Integrations. Create a new webhook integrated with #Channel (target)  
copy the webhook URL and modify the example below for your needs:  
```
discordSend description: "Jenkins Pipeline Build", footer: "Footer Text", link: env.BUILD_URL, result: currentBuild.currentResult, title: JOB_NAME, webhookURL: "Webhook URL"
```

## Acknowledgements

 - [ How to Configure Artifactory in Jenkins ](https://www.youtube.com/watch?v=fj_TD9pufFM)
 - [ How to Integrate SonarQube With Jenkins ](https://www.youtube.com/watch?v=KsTMy0920go)
 - [ Jenkins cleaning up and notifications ](https://www.jenkins.io/doc/pipeline/tour/post/)
 - [ Discord Notifier ](https://plugins.jenkins.io/discord-notifier/)
 - [ Simple Discord Notify from Jenkins ](https://www.linkedin.com/pulse/simple-discord-notify-from-jenkins-edwin-baktian/)

## Badges

![Jenkins](https://img.shields.io/badge/jenkins-%232C5263.svg?style=for-the-badge&logo=jenkins&logoColor=white)

![SonarQube](https://img.shields.io/badge/Sonarqube-5190cf?style=for-the-badge&logo=sonarqube&logoColor=white)

![Apache Maven](https://img.shields.io/badge/Apache%20Maven-C71A36?style=for-the-badge&logo=Apache%20Maven&logoColor=white)

![Artifactory](https://img.shields.io/badge/Artifactory-1997B5&?logo=jfrog&logoColor=white&style=for-the-badge)

![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
