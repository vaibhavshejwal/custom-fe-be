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

	stage('Backend Setup') {
	    steps {
		dir('backend') {
		    sh 'python3 --version'
		    sh 'pip3 install -r requirements.txt'
		    sh 'python3 -m py_compile app.py'
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
