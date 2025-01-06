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


                stage("Deploy & notification"){
                    steps {
                        bat './gradlew publish'
                    }
                          post {
                              success {
                                  emailext subject: "Build Success !",
                                           body: "Good news! The build was successful.",
                                           to: 'la_rabia@esi.dz'
                              }
                          }


                }
}
}
