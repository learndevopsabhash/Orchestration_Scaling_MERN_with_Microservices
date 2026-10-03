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

    }
}
