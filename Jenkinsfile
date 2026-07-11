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

        stage('Skip Jenkins Commit') {
            steps {
                script {
                    def commitMessage = sh(
                        script: "git log -1 --pretty=%B",
                        returnStdout: true
                    ).trim()

                    echo "Latest Commit Message: ${commitMessage}"

                    if (commitMessage.startsWith("Updated image")) {
                        error("Build triggered by Jenkins manifest update. Stopping to avoid infinite loop.")
                    }
                }
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

                    echo "Updated deployment.yaml"

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

        stage('Cleanup Old Images') {
            steps {
                sh '''
                    echo "Cleaning old Docker images..."

                    docker images adityakhiroji/gitops-demo --format "{{.Repository}}:{{.Tag}}" \
                    | grep -E '^[^:]+:[0-9]+$' \
                    | sort -t: -k2 -nr \
                    | tail -n +3 \
                    | xargs -r docker rmi

                    docker image prune -f

                    echo ""
                    echo "Remaining Images:"
                    docker images adityakhiroji/gitops-demo
                '''
            }
        }

        stage('Verify Images') {
            steps {
                sh '''
                    echo ""
                    echo "Docker Images:"
                    docker images

                    echo ""
                    echo "Disk Usage:"
                    docker system df
                '''
            }
        }
    }

    post {
        success {
            echo "=========================================="
            echo "CI/CD Pipeline Completed Successfully"
            echo "Image : ${IMAGE_NAME}:${IMAGE_TAG}"
            echo "ArgoCD will automatically sync the changes."
            echo "=========================================="
        }

        failure {
            echo "=========================================="
            echo "Pipeline Failed!"
            echo "Please check the failed stage."
            echo "=========================================="
        }
    }
}