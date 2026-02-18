pipeline {
    agent any

    environment {
        FRONTEND_IMAGE = 'vaibhavshejwaldocker/frontend-app'
        BACKEND_IMAGE = 'vaibhavshejwaldocker/backend-app'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh "docker build -t ${FRONTEND_IMAGE}:${BUILD_NUMBER} ./frontend"
            }
        }

        stage('Build Backend Image') {
            steps {
                sh "docker build -t ${BACKEND_IMAGE}:${BUILD_NUMBER} ./backend"
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-creds',
                    usernameVariable: 'DOCKER_USERNAME',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    sh """
                        echo "${DOCKER_PASSWORD}" | docker login -u "${DOCKER_USERNAME}" --password-stdin
                        docker push ${FRONTEND_IMAGE}:${BUILD_NUMBER}
                        docker push ${BACKEND_IMAGE}:${BUILD_NUMBER}
                        docker logout
                    """
                }
            }
        }

        stage('Deploy Containers') {
            steps {
                sh """
                    docker stop frontend-container || true
                    docker rm frontend-container || true
                    docker stop backend-container || true
                    docker rm backend-container || true

                    docker pull ${FRONTEND_IMAGE}:${BUILD_NUMBER}
                    docker pull ${BACKEND_IMAGE}:${BUILD_NUMBER}

                    docker run -d --name frontend-container -p 3000:80 ${FRONTEND_IMAGE}:${BUILD_NUMBER}
                    docker run -d --name backend-container -p 5000:5000 ${BACKEND_IMAGE}:${BUILD_NUMBER}
                """
            }
        }
    }

    post {
        success {
            echo "Build and deployment successful."
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
