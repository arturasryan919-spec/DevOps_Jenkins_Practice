pipeline {
	
	agent any
	
	environment {
		IMAGE_NAME = "arturasryan/jenkins-python-app:latest"
	}


	stages {
		
		stage('Build docker image') {

			steps{
				sh '''
				docker build -t jenkins-python-app .
				'''
			}
	
		}

			
		stage('Login to Docker Hub'){
				
			steps{
			    withCredentials([usernamePassword(
				credentialsId: 'dockerhub',
				usernameVariable: 'DOCKER_USER',
				passwordVariable: 'DOCKER_TOKEN'
			    )]) {
				sh '''
				echo "$DOCKER_TOKEN" | docker login -u "$DOCKER_USER" --password-stdin
				'''
			   }
			}

		}

		stage('Tag Image'){

			steps {
				sh '''
				docker tag jenkins-python-app:latest $IMAGE_NAME
				'''
			}			

		}
	
		stage('Push image') {

			steps {
				sh '''
				docker push $IMAGE_NAME
				'''
			}
	
		}
			

		stage('Run container'){

			steps{
				sh '''
				docker stop jenkins-python-container || true
				docker rm jenkins-python-container || true
				
				docker run -d \
				--restart unless-stopped \
				--name jenkins-python-container \
				-p 5000:5000 \
				jenkins-python-app
				'''
				
			}		
	
		}

	}

}
