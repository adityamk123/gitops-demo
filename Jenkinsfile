pipeline {
    agent any

    environment {
        IMAGE_NAME = "adityakhiroji/gitops-demo"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                """
            }
        }

        stage('Docker Images') {
            steps {
                sh 'docker images'
            }
        }
    }
}