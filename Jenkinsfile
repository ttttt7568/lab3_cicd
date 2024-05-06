pipeline {
    agent any

    environment {
        MAIN_IMAGE = 'nodemain:v1.0'
        DEV_IMAGE = 'nodedev:v1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        echo 'Building Docker image for main branch...'
                        sh "docker stop main || true"
                        sh "docker rm main || true"
                        sh "docker build -t $MAIN_IMAGE ."
                        echo 'Docker image built successfully for main branch.'
                    } else if (env.BRANCH_NAME == 'dev') {
                        echo 'Building Docker image for dev branch...'
                        sh "docker stop dev || true"
                        sh "docker rm dev || true"
                        sh "docker build -t $DEV_IMAGE ."
                        echo 'Docker image built successfully for dev branch.'
                    } else {
                        echo 'Branch not recognized. Skipping Docker image build.'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        echo 'Deploying Docker container for main branch...'
                        sh 'docker run -d --expose 3000 -p 3000:3000 --name main $MAIN_IMAGE'
                        echo 'Docker container deployed successfully for main branch.'
                    } else if (env.BRANCH_NAME == 'dev') {
                        echo 'Deploying Docker container for dev branch...'
                        sh 'docker run -d --expose 3001 -p 3001:3000 --name dev $DEV_IMAGE'
                        echo 'Docker container deployed successfully for dev branch.'
                    } else {
                        echo 'Branch not recognized. Skipping Docker container deployment.'
                    }
                }
            }
        }
    }
}
