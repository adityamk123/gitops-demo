pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Environment') {
            steps {
                sh 'pwd'
                sh 'ls -la'
                sh 'docker --version'
                sh 'git --version'
            }
        }
    }
}