pipeline {
    agent any

    stages {

        stage('Build Hello Service') {
            steps {
                sh '''
                    cd backend/helloService
                    docker build -t hello-service:jenkins .
                '''
            }
        }

        stage('Build Profile Service') {
            steps {
                sh '''
                    cd backend/profileService
                    docker build -t profile-service:jenkins .
                '''
            }
        }
		stage('Build Frontend') {
		    steps {
		        sh '''
		            cd frontend
		            docker build -t frontend:jenkins .
		        '''
		    }
		}
		stage('Check AWS ECR Access') {
		steps {
		withCredentials([[
		$class: 'AmazonWebServicesCredentialsBinding',
		credentialsId: 'aws-ecr-jenkins-user'
		]]) {
	            sh '''
		aws sts get-caller-identity
	            '''
	        }
	    }
	}
		stage('Login to ECR') {
		steps {
	        withCredentials([[
		$class: 'AmazonWebServicesCredentialsBinding',
		credentialsId: 'aws-ecr-jenkins-user'
		]]) {
			sh '''
                aws ecr get-login-password --region ap-south-1 | \
                docker login --username AWS --password-stdin 057079472578.dkr.ecr.ap-south-1.amazonaws.com
			'''
		        }
		    }
		}

    }
}
