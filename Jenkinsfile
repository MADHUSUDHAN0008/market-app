pipeline {

    agent any

    environment {
        IMAGE = "lingala89/market-app"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t market-app .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                    )
                ]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    '''
                }
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag market-app $IMAGE:latest'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE:latest'
            }
        }
    }

    post {
        success {
            echo 'Docker image pushed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}