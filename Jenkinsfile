pipeline {
agent any
stages {
stage('Build') {
            steps {
                // Commande de construction sous Windows
                bat './gradlew build'
            }
        }
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