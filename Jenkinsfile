pipeline {

    agent any

    environment {
        IMAGE_NAME = "lingala89/market-app"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                bat '''
                docker build -t %IMAGE_NAME%:%IMAGE_TAG% .
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
                docker push %IMAGE_NAME%:%IMAGE_TAG%
                '''

            }

        }

    }

    post {

        success {

            echo 'SUCCESS'
            bat 'docker logout'

        }

        failure {

            echo 'FAILED'
            bat 'docker logout'

        }

    }

}
