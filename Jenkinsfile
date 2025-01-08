pipeline {
agent any
stages {
 stage('Test') {
     steps {
      

         bat './gradlew test'
         junit '**/build/test-results/test/*.xml'
         archiveArtifacts artifacts: '**/build/test-results/test/*.xml', allowEmptyArchive: true
         cucumber buildStatus: 'UNSTABLE',
                                  reportTitle: 'CucumberReport',
                                  fileIncludePattern: 'reports/example-report.json',
                                  trendsLimit: 10,
                                  classifications: [
                                        [ 
                                             'key': 'Browser',
                                             'value': 'Firefox'
                                        ]
                                  ]
     }

}
 stage("Test Connection") {
    steps {
        script {
            bat 'curl -v http://localhost:9000/api/system/status'
            bat 'curl -v http://localhost:8080'
        }
    }
}
stage("Code Analysis"){
            steps{
                withSonarQubeEnv('sonar') {
                    bat "./gradlew sonar"
                }
            }
        }
 
        stage("Code Quality") {
    steps {
        script {
            timeout(time: 3, unit: 'MINUTES') {
                def qg = waitForQualityGate(webhookSecretId: 'webhook-secret')
                if (qg.status != 'OK') {
                    error "Quality Gate failed: ${qg.status}"
                }
            }
        }
    }
}
        stage("Build")  {
                    steps {
                        bat './gradlew build'
                        bat './gradlew javadoc'
                        archiveArtifacts 'build/libs/*.jar'
                        archiveArtifacts 'build/docs/'
                    }
                }


                stage("Deploy & notification") {
    steps {
        bat './gradlew publish'
    }
    post {
        success {
         
            script {
             slackSend(
                                                  channel: '#sonarqube',
                                                  color: 'good',
                                                  message: 'Deployment avec succés'
                                                  )
                emailext (
                    subject: "Build Success: ${currentBuild.fullDisplayName}",
                    body: """
                        Build succeeded!
                        
                        Project: ${env.JOB_NAME}
                        Build Number: ${env.BUILD_NUMBER}
                        Build URL: ${env.BUILD_URL}
                        
                        Check attached logs for details.
                    """,
                    to: 'ryaaeleanor@gmail.com',
                    mimeType: 'text/html',
                    attachLog: true
                )
                echo "Email notification sent"
            }
        }
        failure {
         
                                              slackSend(
                                              channel: '#sonarqube',
                                              color: 'danger',
                                              message: 'Deployment a échoué '
                                          )
                                      
            emailext (
                subject: "Build Failed: ${currentBuild.fullDisplayName}",
                body: "The build failed. Check the logs for details.",
                to: 'la_rabia@esi.dz'
            )
        }
    }
}
}
}
