pipeline {
	agent any
		environment {
			registry = "prempalsingh/devops"
			registryCredential = "DOCKERHUB_CRED"
			dockerImage = ''
		}
	   stages {
			stage("Build Docker Image") {
				steps {
					script {
						dockerImage = docker.build registry + ":$BUILD_NUMBER"
					}
				}
			}
		    stage("Push Docker Image") {
				steps {
					script {
						docker.withRegistry( '', registryCredential ) {
							dockerImage.push()
						}
					}
				}
			}
		   
		   	stage('Deploy App') {
      			steps {
        			script {
          				sh 'kubectl apply -f  nginxhome.yml'
					}
				}
			}
			
		}
	}