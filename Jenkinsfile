def COLOR_MAP = [
	'SUCCESS': 'good',
	'FAILURE': 'danger',
]

pipeline {
	agent any

	stages {
	    stage('Fetch code') {
		    steps {
		       git branch: 'master', url: 'https://github.com/RushiDonga/test-react.git'
		    }

	    }

	    stage('Build'){
	        steps{
	           sh 'npm install'
	           sh 'npm run build'
	           sh 'zip -r build-V$BUILD_ID.zip build'
	        }
	        
	    	post {
	           success {
	              echo "Now archiving"
	              archiveArtifacts artifacts: '*.zip'
	           }
	        }
	    }
	}
	
	post{
		always{
			echo 'Slack Notification.'
			slackSend channel: '#jenkins-cicd',
				color: COLOR_MAP[currentBuild.currentResult],
				message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n MOre info at: ${}"
		}
	}
}
