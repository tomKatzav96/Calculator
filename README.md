# Jenkins pipeline for Java application
Jenkins pipeline using SonarQube, Maven, and Artifactory for a basic calculator application written in Java, forked from [HouariZegai/Calculator](https://github.com/HouariZegai/Calculator). 

![Image](jenkins-jfrog-maven.png "Architecture of the project")
  
 להוסיף צילום מסך של הפייליין בג'נקינס של השלבים שהצליחו  

 **להגיד שהפרוייקט בנוי על כך שיש גקנקינס מאסטר ואג'נט בנויים כבר.

לחשוב איך לעשות טריגל לפייליין של ג'נקינס מהגיהאב אם בכלל שיהיה טריגר

## Create Artifactory and SonarQube servers

### Artifactory

#### Artifactory quick setup
Launch an instance in AWS use Ubuntu 20.04 image, instance type t3.medium, and add external ports: 8081, 8082.
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

#### Access the JFrog Platform

```url
http://<hostname>:8082
```

For Example: http://localhost:8082 or http://192.168.86.243:8082

The JFrog Platform will take about a minute to start up.

Once the platform is up, log in using username 'admin' and password 'password'.

#### Connect Artifactory with Jenkins
להסביר איך חיברנו בין ארטיפאקטורי וג'נקינס. כולל רישיון וכל מה שצריך וטוקן וזה  
צריך לעשות רישיון עדיין אל עשיתי

### SonarQube
#### Installing an instance of SonarQube with Docker image

Launch an instance in AWS use Ubuntu 22.04 image, instance type t2.medium, and add external port: 9000.   
  
Before you continu make sure you have docker installed in this instane


1. Start the Docker container by running:

```
docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:latest
```
2. update restart policy to always
```
 docker update --restart always sonarqube
 ```
#### Access the SonarQube Platform

```url
http://<hostname>:8082
```

For Example: http://localhost:8082 or http://192.168.86.243:8082

Once the platform is up, log in using username 'admin' and password 'admin'.

#### Connect SonarQube with Jenkins
להסביר איך חיברנו בין סונארקיוב וג'נקינס. כולל רישיון וכל מה שצריך וטוקן וזה

## Create E-mail Notification

להסביר איך יצרתי חיבור של שליחת הודעה מג'נקינס לג'ימייל, וכל זה
עדיין לא עשיתי צריך לעשות
