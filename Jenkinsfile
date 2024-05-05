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
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh "docker stop main || true"
                        sh "docker rm main || true"
                        sh "docker build -t $MAIN_IMAGE ."
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh "docker stop dev || true"
                        sh "docker rm dev || true"
                        sh "docker build -t $DEV_IMAGE ."
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                // Run Docker container based on branch
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh 'docker run -d --expose 3000 -p 3000:3000 --name main $MAIN_IMAGE'
                    } else if (env.BRANCH_NAME == 'dev') {
                        sh 'docker run -d --expose 3001 -p 3001:3000 --name dev $DEV_IMAGE'
                    }
                }
            }
        }
    }
}
