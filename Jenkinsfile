pipeline {
agent any
stages {
 stage('Test') {
     steps {
         bat './gradlew test'
     }
}
stage('Archive Test Results') {
            steps {
                junit '**/build/test-results/test/*.xml'
                archiveArtifacts artifacts: '**/build/test-results/test/*.xml', allowEmptyArchive: true
            }
        }

}
}