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
    
      docker.build("shabanaat/spring-petclinic:${env.BUILD_NUMBER")
    }
        
}
