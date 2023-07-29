      pipeline {
        agent any
        environment {
          CI = true
          ARTIFACTORY_ACCESS_TOKEN = credentials('artifactory-access-token')
          FAIL_TITLE = "Failed Pipeline: ${JOB_NAME}"
          FAIL_DESCRIPTION = "Something is wrong with build number ${BUILD_DISPLAY_NAME}"
          SUCCESS_TITLE = "Successful Pipeline: ${JOB_NAME}"
          SUCCESS_DESCRIPTION = "The pipeline ${JOB_NAME} completed successfully."
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
                discordSend description: FAIL_DESCRIPTION, footer: "Stage: Build & SonarQube Analysis", link: env.BUILD_URL, result: currentBuild.currentResult, title: FAIL_TITLE , webhookURL: 'https://discord.com/api/webhooks/1132648511058497556/8yRNdxJ_9jY4-QDZIbotxpufmbzvgTf9MZSm0OUSgid9ri72yfPtQ-NFLDEo7LECbRC9'
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
                discordSend description: FAIL_DESCRIPTION, footer: "Stage: Quality Gate", link: env.BUILD_URL, result: currentBuild.currentResult, title: FAIL_TITLE , webhookURL: 'https://discord.com/api/webhooks/1132648511058497556/8yRNdxJ_9jY4-QDZIbotxpufmbzvgTf9MZSm0OUSgid9ri72yfPtQ-NFLDEo7LECbRC9'
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
              sh 'echo Upload To Artifactory'
              sh 'jfrog rt upload --url http://172.31.34.51:8082/artifactory/ --access-token ${ARTIFACTORY_ACCESS_TOKEN} target/Calculator-1.0-SNAPSHOT.jar java-calculator/'
            }
            post { 
              failure  { 
                echo 'Upload To Artifactory stage fail'
                discordSend description: FAIL_DESCRIPTION, footer: "Stage: Upload To Artifactory", link: env.BUILD_URL, result: currentBuild.currentResult, title: FAIL_TITLE , webhookURL: 'https://discord.com/api/webhooks/1132648511058497556/8yRNdxJ_9jY4-QDZIbotxpufmbzvgTf9MZSm0OUSgid9ri72yfPtQ-NFLDEo7LECbRC9'
              }
            }
          }
        }
        post {
           success {
                echo 'Pipeline succeeded!'
                discordSend description: SUCCESS_DESCRIPTION, footer: "Keep going", link: env.BUILD_URL, result: currentBuild.currentResult, title: SUCCESS_TITLE , webhookURL: 'https://discord.com/api/webhooks/1132648511058497556/8yRNdxJ_9jY4-QDZIbotxpufmbzvgTf9MZSm0OUSgid9ri72yfPtQ-NFLDEo7LECbRC9'
            }
          }
        }