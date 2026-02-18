pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'docker-creds'
        DOCKERHUB_REPO_FRONTEND = 'vaibhavshejwal/frontend-app'
        DOCKERHUB_REPO_BACKEND = 'vaibhavshejwal/backend-app'
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
                    docker.build("${DOCKERHUB_REPO_FRONTEND}:${BUILD_NUMBER}", "./frontend")
                }
            }
        }

        stage('Build Backend Image') {
            steps {
                script {
                    docker.build("${DOCKERHUB_REPO_BACKEND}:${BUILD_NUMBER}", "./backend")
                }
            }
        }

        stage('Push Images') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        docker.image("${DOCKERHUB_REPO_FRONTEND}:${BUILD_NUMBER}").push()
                        docker.image("${DOCKERHUB_REPO_BACKEND}:${BUILD_NUMBER}").push()
                    }
                }
            }
        }
    }
}

