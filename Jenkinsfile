pipeline {
    agent any

    environment {
        DOCKER_USER = 'vaibhavshejwal'
        FRONTEND_IMAGE = 'vaibhavshejwal/frontend-app'
        BACKEND_IMAGE = 'vaibhavshejwal/backend-app'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Frontend Image') {
            steps {
                script {
                    sh """
                        docker build -t ${FRONTEND_IMAGE}:${BUILD_NUMBER} ./frontend
                    """
                }
            }
        }

        stage('Build Backend Image') {
            steps {
                script {
                    sh """
                        docker build -t ${BACKEND_IMAGE}:${BUILD_NUMBER} ./backend
                    """
                }
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
    }

    post {
        success {
            echo "Images pushed successfully with tag ${BUILD_NUMBER}"
        }
        failure {
            echo "Build failed. Check logs."
        }
    }
}

