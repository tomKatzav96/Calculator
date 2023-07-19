# Jenkins pipeline for Java application
Jenkins pipeline using SonarQube, Maven, and Artifactory for a basic calculator application created using Java Swing that forked from [HouariZegai/Calculator](https://github.com/HouariZegai/Calculator). 


## Prerequisites

create Jenkins, Artifactory and SonarQube servers

### Create Jenkins Master and Agent servers
....



### Create Artijactory server

#### Artifactory quick setup
Launch an instance ins AWS use Ubuntu image 20.04, instance type t3.medium, and add external ports: 8081, 8082.
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
