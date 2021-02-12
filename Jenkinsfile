def provider = 'gcp'
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
                expression { params.ACTION == 'true' }
               }
              steps {
	         sh  " sh ${script} ${provider}"
                    }
          }

           
    }
}
	    
