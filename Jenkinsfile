#!groovy

pipeline {
  agent none
  stages {
    stage('Maven Install') {
      agent {
        docker {
          image 'maven:3.5.3'
        }
      }
      steps {
        sh 'mvn clean install'
      }
    } 
  }
}
 
 stage('Docker Push') {
      agent any
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'shabanaa21', usernameVariable: 'shabanaat')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push shabanaat/spring-petclinic:latest'
        }
      }
    }
  
 

