pipeline {
    agent any

    stages {

        stage('Check Docker') {
            steps {
                bat '''
                docker --version
                docker info
                docker ps
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '''
                docker build -t market-app:test .
                '''
            }
        }

        stage('List Images') {
            steps {
                bat '''
                docker images
                '''
            }
        }

    }

    post {
        success {
            echo 'Docker is working correctly.'
        }
        failure {
            echo 'Docker test failed.'
        }
    }
}