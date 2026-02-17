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
                    image 'maven:3.9.6-eclipse-temurin-17'
                    args '-v $WORKSPACE:/app -w /app/backend'
                }
            }
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build Frontend') {
            agent {
                docker {
                    image 'node:20-bullseye'
                    args '-v $WORKSPACE:/app -w /app/frontend'
                }
            }
            steps {
                sh 'npm install'
                sh 'npm run build -- --prod'
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

