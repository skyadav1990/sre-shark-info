pipeline {
	agent any
		environment {
			project = "shark-info"
			registry = "prempalsingh/devops"
			registryCredential = "DOCKERHUB_CRED"
			dockerImage = ''
			namespace = ${project}
			
			
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
		   
		   	stage('Deploy Application') {
      			steps {
        			script {
						sh("kubectl get ns ${namespace} || kubectl create ns ${namespace}")
						sh("kubectl config set-context --current --namespace=${namespace}")
          				sh("kubectl apply -f deploy.yml")
					}
				}
			}
			
		}
	}