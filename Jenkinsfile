pipeline {
    agent any

    environment {
        IMAGE_NAME_BACKEND = "pip-tracker-backend"
        IMAGE_NAME_FRONTEND = "pip-tracker-frontend"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Build & Test Backend') {
            agent {
                docker {
                    image 'maven:3.9.3-openjdk-20'
                    args '-v $PWD:/app'
                }
            }
            steps {
                dir('backend') {
                    sh 'mvn clean test'
                }
            }
        }

        stage('Build Frontend') {
            agent {
                docker {
                    image 'node:20-bullseye'
                    args '-v $PWD:/app'
                }
            }
            steps {
                dir('frontend') {
                    sh 'npm install'
                    sh 'npm run build -- --prod'
                }
            }
        }

        stage('Build Docker Images') {
            steps {
                sh """
                    docker build -t ${IMAGE_NAME_BACKEND}:${TAG} ./backend
                    docker build -t ${IMAGE_NAME_FRONTEND}:${TAG} ./frontend
                """
            }
        }

        stage('Deploy Containers') {
            steps {
                sh '''
                    docker-compose down
                    docker-compose up -d --build
                '''
            }
        }
    }

    post {
        success {
            echo "Application deployed successfully 🚀"
        }
        failure {
            echo "Build failed ❌ Deployment skipped"
        }
    }
}

