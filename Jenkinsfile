node{
        stage('Initialize'){
                def mvnHome = tool 'Mymaven'
    env.PATH = "${mvnHome}/bin:${env.PATH}"
        }                
        
stage('Checkout') {
        checkout scm
    }
        stage('Build'){
                sh 'mvn clean install'
        }
        
    stage('Create Docker Image') {
    
            docker.build("shabanaat/spring-petclinic:${env.BUILD_NUMBER}")
    }
        
        stage('Push to Docker Registry'){
          withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
          sh 'docker push shabanaat/spring-petclinic:latest'
     }
}
        stage('Dev-Deploy'){
                sh ‘ssh root@192.168.91.59’
                sh 'docker pull shabanaat/spring-petclinic'
                sh 'docker run -d -t 8050:8080 shabanaat/spring-petclinic'
               
}

