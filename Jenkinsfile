pipeline {
 agent any
stages {
 stage ('build') {    // la phase build
 steps {
      bat "./gradlew build"
      archiveArtifacts 'build/libs/*.jar'
}
 stage ('test') {
steps {
    bat "./gradle test"
}
 }
 }