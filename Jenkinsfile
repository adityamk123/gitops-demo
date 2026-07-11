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

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh """
                    docker push ${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }

        stage('Update deployment.yaml') {
            steps {
                sh """
                    sed -i 's|image: .*|image: ${IMAGE_NAME}:${IMAGE_TAG}|' k8s/deployment.yaml
                    cat k8s/deployment.yaml
                """
            }
        }

        stage('Commit Changes') {
            steps {
                sh '''
                    git config user.name "Jenkins"
                    git config user.email "jenkins@local"

                    git add k8s/deployment.yaml

                    git diff --cached --quiet || git commit -m "Updated image to ${BUILD_NUMBER}"
                '''
            }
        }

        stage('Push Changes') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'github',
                    usernameVariable: 'GIT_USER',
                    passwordVariable: 'GIT_TOKEN'
                )]) {
                    sh '''
                        git push https://${GIT_USER}:${GIT_TOKEN}@github.com/adityamk123/gitops-demo.git HEAD:main
                    '''
                }
            }
        }

        stage('Verify Images') {
            steps {
                sh 'docker images'
            }
        }
    }
}