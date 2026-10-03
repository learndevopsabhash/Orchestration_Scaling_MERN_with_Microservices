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

    }
}
