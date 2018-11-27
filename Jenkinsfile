pipeline {
    environment {
        registry = "nexus.sysdata.it:18000/getintodevops-hellonode"
        registryCredential = 'nexus-credentials'
        dockerImage = ''
    }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        checkout scm
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( registry, registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
  }
}