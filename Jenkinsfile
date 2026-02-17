pipeline {
    agent any

    stages {
	stage('Checkout code') {
	    steps {
		git branch: 'main', url: 'https://github.com/vaibhavshejwal/custom-fe-be.git'
	    }
	}

	stage('Build Frontend') {
	    steps {
		dir('frontend') {
		    sh 'npm install'
		    sh 'npm run build'
		}
	    }
	}
    }
}
