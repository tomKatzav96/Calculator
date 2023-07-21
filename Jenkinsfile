      pipeline {
        agent any
        environment {
          CI = true
          ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
        }
        stages {
          stage("Build & SonarQube Analysis") {
            steps {
              withSonarQubeEnv(installationName: 'sq1') {
                sh 'mvn clean install sonar:sonar'
              }
            }
            post { 
              failure  { 
                echo 'build & SonarQube analysis stage fail'
                mail to: 'tom.katzav@gmail.com',
                  subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                  body: "Something is wrong with ${env.BUILD_URL}"
              }
            }
          }
          stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
            post { 
              failure  { 
                echo 'Quality Gate stage fail'
                mail to: 'tom.katzav@gmail.com',
                  subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                  body: "Something is wrong with ${env.BUILD_URL}"
              }
            }
          }
          stage("Upload To Artifactory") {
            agent {
              docker {
                image 'releases-docker.jfrog.io/jfrog/jfrog-cli-v2:2.2.0'
                reuseNode true
              }
            }
            steps {
              sh 'jfrog rt upload --url http://172.31.34.51:8082/artifactory/ --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/Calculator-1.0-SNAPSHOT.jar java-calculator/'
            }
            post { 
              failure  { 
                echo 'Upload To Artifactory stage fail'
                mail to: 'tom.katzav@gmail.com',
                  subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                  body: "Something is wrong with ${env.BUILD_URL}"
              }
            }
          }
        }
        post {
           success {
                echo 'Pipeline succeeded!'
                mail to: 'tom.katzav@gmail.com',
                  subject: "Succeeded Pipeline: ${currentBuild.fullDisplayName}",
                  body: "The pipeline ${currentBuild.fullDisplayName} completed successfully."
            }
          }
        }
      }