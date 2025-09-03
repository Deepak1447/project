pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-hub') // replace with your Jenkins credential ID
        IMAGE_NAME = "deepak-html-ui" // choose your image name
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh """
                        docker build -t $DOCKERHUB_CREDENTIALS_USR/$IMAGE_NAME:${env.BUILD_NUMBER} .
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    sh """
                        echo "$DOCKERHUB_CREDENTIALS_PSW" | docker login -u "$DOCKERHUB_CREDENTIALS_USR" --password-stdin
                        docker push $DOCKERHUB_CREDENTIALS_USR/$IMAGE_NAME:${env.BUILD_NUMBER}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Docker image pushed successfully: $DOCKERHUB_CREDENTIALS_USR/$IMAGE_NAME:${env.BUILD_NUMBER}"
        }
        failure {
            echo "Build failed!"
        }
    }
}

