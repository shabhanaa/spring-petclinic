node{
        stage('Initialize'){
                def mvnHome = tool 'Mymaven'
                //def sshHost = '192.168.91.59'
    env.PATH = "${mvnHome}/bin:${env.PATH}"
        }                
        
stage('Checkout') {
        checkout scm
    }
        stage('Build'){
                sh 'mvn clean install'
        }
        
       stage('Sonar'){
        try {
            sh "mvn sonar:sonar"
        } catch(error){
            
        }
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
        stage('DeployToDev'){
        sh 'docker stop spring-petclinic || true && docker rm spring-petclinic || true'
        sh 'docker run --name spring-petclinic -d -p 8050:8080 shabanaat/spring-petclinic
    }
       
}

