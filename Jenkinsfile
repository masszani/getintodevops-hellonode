def buildNum = "${env.BUILD_NUMBER}"

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
          docker.withRegistry( "http://nexus.sysdata.it:18000", registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy K8S') {
        steps {
         /*   withKubeConfig([credentialsId:'kubeconfig']) {
                sh("sed -i.bak 's#latest#${buildNum}#' ./K8S/*.yaml")
                sh("kubectl --namespace=production apply -f K8S/")
            } */
          withCredentials([kubeconfigContent(credentialsId: 'kubeconfig', variable: 'KUBECONFIG_CONTENT')]){
            sh '''echo "$KUBECONFIG_CONTENT" > kubeconfig && cat kubeconfig && rm kubeconfig'''
          }
        }
    }
    
  }
}