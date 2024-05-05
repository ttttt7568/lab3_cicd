pipeline {
    agent any

    environment {
        MAIN_IMAGE = 'nodemain:v1.0'
        DEV_IMAGE = 'nodedev:v1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Install dependencies for NodeJS application
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                // Test NodeJS application
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build Docker image for main branch
                sh 'echo "$env.BRANCH_NAME"'
                sh 'echo $env.BRANCH_NAME'                                
                sh 'echo $MAIN_IMAGE'
                sh 'docker build -t test12345 .'               
            }
        }

        stage('Deploy') {
            steps {
                // Run Docker container based on branch
                sh 'docker run -d --expose 3000 -p 3000:3000 test12345'
            }
        }
    }
}
