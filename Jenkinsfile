def provider = 'aws'
// Conditionally define a variable 'impact'
if (provider == 'aws') {
  script = "aws_bash.sh"
} 
else {
  script = "gcp_bash.sh"
}

pipeline {
  agent any
	environment {
        DOCKER_IMAGE_NAME = "shuvamoy008/sparkops"
	workspace = "${env.WORKSPACE}"
	
    }
	 parameters {
        choice(
            choices: 'true\nfalse',
            description: 'Select ? ',
            name: 'ACTION')
    }
	
    
    stages {
	   
	   stage ("Running Terraform for Cloud Provisioning") {
	      when {
                expression { params.ACTION == 'false' }
               }
              steps {
	         sh  " sh ${script} ${provider}"
                    }
          }

           
    
         stage('Build Docker Image') {
            when {
                expression { params.ACTION == 'true'}
            }
            steps {
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
		    
                }
            }
        }
        stage('Push Docker Image') {
            when {
                 expression { params.ACTION == 'true' }
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') 
			{
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
			sh "sleep 60"
                        }
                     }
                 }
         }
    }
}
	    


