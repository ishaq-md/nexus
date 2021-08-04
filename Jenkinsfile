pipeline {
    agent any
	
	  tools
    {
       maven "Maven.3.6.3"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/ishaqmdgcp/nexus.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
                
                sh 'docker build -t samplewebapp:latest .' 
                sh 'docker tag samplewebapp ishaqmd/javaapp:latest'
               	                 
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
        
		withCredentials([string(credentialsId: 'DOCKER_USER', variable: 'DOCKER_USER'), string(credentialsId: 'PASSWD', variable: 'DOCKER_PASSWORD')]) {
            sh "docker login -u $DOCKER_USER -p $DOCKER_PASSWORD"
              sh  'docker push ishaqmd/javaapp:latest'
          }
		  

}
        
                  
          
  }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run --name javaapp -d -p 8008:8080 ishaqmd/javaapp"
				sh 'curl localhost:8008/hello'
				sh 'sleep 20'
				sh 'docker stop javaapp'
				sh 'docker rm javaapp'
 
            }
        }
 
    }
	}
