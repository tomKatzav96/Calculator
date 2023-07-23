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
                discordSend description: 'Something is wrong with ${env.BUILD_URL}', footer: '', image: '', link: '', result: '', scmWebUrl: '', thumbnail: '', title: 'Failed Pipeline: ${currentBuild.fullDisplayName}', webhookURL: 'https://discord.com/api/webhooks/1132648511058497556/8yRNdxJ_9jY4-QDZIbotxpufmbzvgTf9MZSm0OUSgid9ri72yfPtQ-NFLDEo7LECbRC9'              }
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
                discordSend description: 'Something is wrong with ${env.BUILD_URL}', footer: '', image: '', link: '', result: '', scmWebUrl: '', thumbnail: '', title: 'Failed Pipeline: ${currentBuild.fullDisplayName}', webhookURL: 'https://discord.com/api/webhooks/1132648511058497556/8yRNdxJ_9jY4-QDZIbotxpufmbzvgTf9MZSm0OUSgid9ri72yfPtQ-NFLDEo7LECbRC9'              }
            }
          }
          // stage("Upload To Artifactory") {
          //   agent {
          //     docker {
          //       image 'releases-docker.jfrog.io/jfrog/jfrog-cli-v2:2.2.0'
          //       reuseNode true
          //     }
          //   }
          //   steps {
          //     sh 'jfrog rt upload --url http://172.31.34.51:8082/artifactory/ --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/Calculator-1.0-SNAPSHOT.jar java-calculator/'
          //   }
          //   post { 
          //     failure  { 
          //       echo 'Upload To Artifactory stage fail'
          //       mail to: 'tom.katzav@gmail.com',
          //         subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
          //         body: "Something is wrong with ${env.BUILD_URL}"
          //     }
          //   }
          // }
        }
        post {
           success {
                echo 'Pipeline succeeded!'
                discordSend description: 'The pipeline ${currentBuild.fullDisplayName} completed successfully.', footer: '', image: '', link: '', result: '', scmWebUrl: '', thumbnail: '', title: 'Success Pipeline: ${currentBuild.fullDisplayName}', webhookURL: 'https://discord.com/api/webhooks/1132648511058497556/8yRNdxJ_9jY4-QDZIbotxpufmbzvgTf9MZSm0OUSgid9ri72yfPtQ-NFLDEo7LECbRC9'
            }
          }