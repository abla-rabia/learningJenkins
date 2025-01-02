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

}
}