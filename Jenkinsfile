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
stage("Code Analysis"){
            steps{
                withSonarQubeEnv('sonar') {
                    bat "./gradlew sonar"
                }
            }
        }
        stage("Code Quality") {
                    steps {
                        waitForQualityGate abortPipeline: true
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
                                  emailext subject: "Build Success !",
                                           body: "Good news! The build was successful.",
                                           to: 'la_rabia@esi.dz'
                              }
                          }


                }
}
}