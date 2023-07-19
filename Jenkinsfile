      pipeline {
        agent any
        environment {
          CI = true
          ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
        }
        stages {
          stage("build & SonarQube analysis") {
            steps {
              withSonarQubeEnv(installationName: 'sq1') {
                sh 'mvn clean install sonar:sonar'
              }
            }
          }
          stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
          stage("Upload to Artifactory") {
            steps {
              sh 'jfrog rt upload --url http://172.31.34.51:8082/artifactory/ --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/Calculator-1.0-SNAPSHOT.jar java-calculator/'
            }
          }
        }
      }