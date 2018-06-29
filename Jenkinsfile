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
        stage('Junit')
	{
        try {
            sh "mvn test" 
	    } catch(error)
	    {
            echo "The Maven can not perform Junit ${error}"
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

        stage('DeployToQA'){
        sh 'docker stop spring-petclinic || true && docker rm spring-petclinic || true'
        sh 'docker run --name spring-petclinic -d -p 9010:8080 shabanaat/spring-petclinic'
                
        }
  
        stage('Checkout multi branch'){
                git([url: 'https://github.com/shabhanaa/spring-petclinic.git', branch: "${env.BRANCH_NAME}", credentialsId: "${env.CREDENTIALS_ID}"])
}
}

