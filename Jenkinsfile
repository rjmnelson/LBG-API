pipeline {
	agent any
	stages{
		stage('Build Image'){
			steps{
			sh 'docker build -t gcr.io/lbg-210222/api-rhid:latest -t gcr.io/lbg-210222/api-rhid:build-$BUILD_NUMBER .'
			}
		}
		stage('Push GCR'){
			steps{
            sh 'docker push gcr.io/lbg-210222/api-rhid:build-$BUILD_NUMBER'
			sh 'docker push gcr.io/lbg-210222/api-rhid:latest'
			}
		}
		stage('Reapply '){
			steps{
			sh '''kubectl apply -f ./kubernetes/nginx.yaml
            kubectl apply -f ./kubernetes/api-deployment.yaml
			kubectl rollout restart deployment/api
			'''
			}
		}
        stage('Cleanup'){
			steps{
            sh 'docker rmi gcr.io/lbg-210222/api-rhid:latest'
			sh 'docker rmi gcr.io/lbg-210222/api-rhid:build-$BUILD_NUMBER'
			}
		}
    }
}
