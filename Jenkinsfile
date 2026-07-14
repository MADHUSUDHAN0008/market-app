pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "lingala89/market-app"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Checkout Source') {
            steps {
                checkout scm
            }
        }

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
                docker build -t %DOCKER_IMAGE%:%IMAGE_TAG% .
                '''
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )
                ]) {
                    bat '''
                    @echo off
                    echo Logging into Docker Hub...
                    echo %DOCKER_PASSWORD% | docker login -u %DOCKER_USERNAME% --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat '''
                docker push %DOCKER_IMAGE%:%IMAGE_TAG%
                '''
            }
        }

        stage('Verify Image') {
            steps {
                bat '''
                docker images
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
            bat 'docker logout'
        }

        failure {
            echo 'Pipeline failed.'
            bat 'docker logout'
        }

        always {
            cleanWs()
        }
    }
}