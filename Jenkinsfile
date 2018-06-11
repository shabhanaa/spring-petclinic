#!grovy

pipeline {
  agent none
  stages {
    stage('Maven Install') {
      agent {
        docker {
          image 'maven:3.5.0'
        }
      }
      steps {
        sh 'mvn clean install'
      }
    }
    stage('Docker Build') {
      agent any
      steps {
        sh 'docker build -t shabanaat/spring-petclinic:latest .'
      }
    }
  }
}

 stage('Docker Push') {
         agent any
      steps {
        withCredentials( [ (credentialsId:'dockerHub', passwordVariable:'shabanaa21', usernameVariable:'shabanaat')] ) {
          sh "docker login -u shabanaat -p shabanaa21"
          sh 'docker push shabanaat/spring-petclinic:latest'
        
      }
	}
    
 }
 

