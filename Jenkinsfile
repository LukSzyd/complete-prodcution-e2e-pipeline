pipeline {
    agent {
        label "jenkins-agent"
    }
	environment{
		APP_NAME = "complete-prodcution-e2e-pipeline"
	}
	
    stages {
        stage('Cleanup Workspace') { 
            steps {
                cleanWs();
            }
        }
		
		stage("Checkout from SCM"){
            steps {
                git branch: 'master', credentialsId: 'github', url: 'https://github.com/LukSzyd/complete-prodcution-e2e-pipeline'
            }
        }
		
		
		stage("Update the deployment Tags"){
            steps {
				sh """
					cat deployment.yaml
					sed -i 's/${APP_NAME}.*/${APP_NAME}:${APP_NAME}/g' deployment.yaml
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
				withCredentials([gitUsernamePassword(credentialsId: 'github—pat', gitToolname: 'Default')]) {
					sh "git push https://github.com/LukSzyd/complete-prodcution-e2e-pipeline master" 
				}
			}
		}
}
