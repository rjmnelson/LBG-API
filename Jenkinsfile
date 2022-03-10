pipeline {
	agent any
	stages{
		stage('Build Image'){
			steps{
			sh 'docker build -t rjmn1976/api:build-$BUILD_NUMBER .'
			}
		}
		stage('Push to Dockerhub'){
			steps{
            sh 'docker push rjmn1976/api:build-$BUILD_NUMBER'
			}
		}
		stage('Reapply '){
			steps{
			sh '''kubectl apply -f ./kubernetes/nginx.yaml
            kubectl apply -f ./kubernetes/api-deployment.yaml
			'''
			}
		}
        stage('Cleanup'){
			steps{
            sh 'docker system prune'
			}
		}
    }
}
