pipeline {
	agent any
	stages {
		stage("Checkout code") {
			steps {
				checkout scm
			}
		}


	// Develop chain

		stage('Build image with tag latest for develop branch') {
			when{
				expression{env.GIT_BRANCH == 'origin/main'}
			}
			steps{
				script {
					ms1 = docker.build("deep-test-295508/ms1","-f ./cicd/Dockerfile ./ ")
				}
			}
		}

		stage('Push image with tag latest for main branch') {
			when{
				expression{env.GIT_BRANCH == 'origin/main'}
			}
			steps{
				script {
					docker.withRegistry('https://us.gcr.io', 'gcr:jenkins-gcr-account') {
						ms1.push("latest")
					}                    
				}
			}
		}

		stage ('Deploy image on dev cluster') {
			when{
				expression{env.GIT_BRANCH == 'origin/main'}
			}
			steps{
				build job: 'deploy-to-k8s'
			}    
		}
	}
}
