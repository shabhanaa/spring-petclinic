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
        sh 'docker run --name spring-petclinic -d -p 9050:8080 shabanaat/spring-petclinic'
                
        }

        
   stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('http://192.168.91.59', 'docker-hub-credentials'){
                sh 'docker run --name spring-petclinic -d -p 'http://192.168.91.59' shabanaat/spring-petclinic'


    }
	}
}
}

