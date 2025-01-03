pipeline {
agent any
stages {
 stage('Test') {
     steps {
     step{
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
     }}

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
        stage("Build") {
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
                               notifyEvents message: 'Success ',
                               mail to: 'la_rabia@esi.dz',
                               subject: "Success",
                               body: "Deployment successful"
                          }
                          failure {
                                notifyEvents message: 'Failure',
                                mail to: 'la_rabia@esi.dz',
                                subject: "Failure",
                                body: "Something went wrong "
                          }
                    }
                }
}
}