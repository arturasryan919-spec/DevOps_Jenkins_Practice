pipeline {
	
	agent any
	
	stages {
	
		stage('Build docker image') {

			steps{
				sh '''
				docker build -t jenkins-python-app .
				'''
			}
	
		}

		stage('Run container'){

			steps{
				sh '''
				docker stop jenkins-python-container || true
				docker rm jenkins-python-container || true
				
				docker run -d \
				--name jenkins-python-container \
				-p 5000:5000 \
				jenkins-python-app
				'''
				
			}		
	
		}

}
