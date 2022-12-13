def COLOR_MAP = [
	'SUCCESS': 'good',
	'FAILURE': 'danger',
]

pipeline {
	agent any
	
	environment {
		registryCredential = 'ecr:ap-south-1:AWSCRED'
		appRegistry = "283227483309.dkr.ecr.ap-south-1.amazonaws.com/vprofileapp"
		vprofileRegistry = "https://283227483309.dkr.ecr.ap-south-1.amazonaws.com"
        }

	stages {
		stage('Fetch code') {
			steps {
				git branch: 'AWS-Setup-Tomcat', url: 'https://github.com/RushiDonga/Jenkins-CICD.git'
			}
		}
	    
		stage('Build App Image') {
			steps {
	       
		 		script {
		        		dockerImage = docker.build( appRegistry + ":$BUILD_NUMBER", "./Docker-files/app/multistage/")
		     		}
	     		}
	    	}
	    	
	    	stage('Upload App Image') {
			steps{
		    		script {
		     	 		docker.withRegistry( vprofileRegistry, registryCredential ) {
		        			dockerImage.push("$BUILD_NUMBER")
		        			dockerImage.push('latest')
		      			}
		    		}
		  	}
	     	}
	}
     	
	post{
		always{
			echo 'Slack Notification.'
			slackSend channel: '#image',
				color: COLOR_MAP[currentBuild.currentResult],
				message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n This is a Test Notification"
		}
	}
}
