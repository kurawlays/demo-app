pipeline {
    agent any
    stages {

    // Tests
   /* stage('Unit Tests') {
      steps{
        script {
          sh 'npm install'
	  sh 'npm test -- --watchAll=false'
        }
      }
    }*/
        
    // Building Docker images
    stage('Building image') {
      steps{
        script {
	  echo "The build number is ${env.BUILD_NUMBER}"
    //      def dockerImage = sudo docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}" 
	          dockerImage = sudo docker.build "demo-app:1"
        }
      }
    }
	    
// hello
   
    // Uploading Docker images into AWS ECR
    stage('Pushing to ECR') {
     steps{  
         script {
			 docker.withRegistry("https://191856567065.dkr.ecr.us-east-1.amazonaws.com/demo" + registryCredential) {
                    	 dockerImage.push()
                	}
         }
        }
      }
      
    stage('Deploy') {
     steps{
            withAWS(credentials: registryCredential, region: "${AWS_DEFAULT_REGION}") {
                script {
			 sh './script.sh'
                }
            } 
        }
      }      
      
    }
}
