pipeline {
	agent any
		environment {
			project = "shark-info"
			registry = "dockerhub-skyadav1990/registry-name"
			registryCredential = "DOCKERHUB_CRED"
			dockerImage = ''
			namespace = "${project}"
			imageTag = "${BUILD_NUMBER}"
			
			
		}
	   stages {
			stage("Build Docker Image") {
				steps {
					script {
						dockerImage = docker.build registry + ":$imageTag"
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
						sh("sed -i.bak 's#${registry}:latest#${registry}:${imageTag}#' deploy.yml")
						sh("kubectl get ns ${namespace} || kubectl create ns ${namespace}")
						sh("kubectl --namespace=${namespace} apply -f deploy.yml")
					}
				}
			}
			
		}
	}
