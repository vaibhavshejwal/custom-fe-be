pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('frontend') {
                    sh 'npm install'
                }
            }
        }

        stage('Build') {
            steps {
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }

        stage('Verify Build Output') {
            steps {
                dir('frontend') {
                    sh 'ls -la build'
                }
            }
        }

        stage('Archive Build') {
            steps {
                archiveArtifacts artifacts: 'frontend/build/**', fingerprint: true
            }
        }
    }
}
