      pipeline {
        agent any
        stages {
          stage("build & SonarQube analysis") {
            steps {
              withSonarQubeEnv(installationName: 'sq1') {
                sh 'mvn clean package sonar:sonar'
              }
            }
          }
        }
      }