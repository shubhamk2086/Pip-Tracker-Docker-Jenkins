pipeline {
    agent any

    environment {
        IMAGE_NAME_BACKEND = "pip-tracker-backend"
        IMAGE_NAME_FRONTEND = "pip-tracker-frontend"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Install Tools') {
            steps {
                sh '''
                    echo "Updating packages..."
                    sudo apt-get update -y

                    echo "Installing Maven..."
                    sudo apt-get install maven -y
                    mvn -version

                    echo "Installing Node.js and npm..."
                    sudo apt-get install nodejs npm -y
                    node -v
                    npm -v
                '''
            }
        }

        stage('Build & Test Backend') {
            steps {
                dir('backend') {
                    sh 'mvn clean test'
                }
            }
        }

        stage('Build Frontend') {
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

