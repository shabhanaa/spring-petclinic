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
        sh 'sudo docker stop spring-petclinic || true && sudo docker rm spring-petclinic || true'
        sh 'sudo docker run --rm --memory="1400m" --cpus=0.250 --name spring-petclinic -d -p 8085:8080 shabanaat/$JOB_BASE_NAME:$BUILD_ID'
    }
       
}

