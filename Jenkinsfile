pipeline {
    agent {
        label "testowy-agent"
    }
	environment{
		APP_NAME = "complete-prodcution-e2e-pipeline-github-actions"
	}
	
    stages {
        stage('Cleanup Workspace') { 
            steps {
                cleanWs();
            }
        }
		
		stage("Checkout from SCM"){
            steps {
                git branch: 'master', credentialsId: 'github_credentials', url: 'https://github.com/LukSzyd/complete-prodcution-e2e-pipeline'
            }
        }
		
		
		stage("Update the deployment Tags"){
            steps {
				sh """
					cat deployment.yaml
					sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
					cat deployment.yaml
				"""
			}
		}
		
		stage("Push updated file to GIT"){
            steps {
				sh """
					git config --global user.name "LukSzyd"
					git config --global user.email "szydlowski333@gmail.com"
					git add deployment.yaml
					git commit -m "Updated Deployment Manifest"
				"""
				withCredentials([usernamePassword(credentialsId: 'github_credentials', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh('git push https://${GIT_PASSWORD}@github.com/LukSzyd/complete-prodcution-e2e-pipeline.git master')
                }
			}
		}
	}
}
