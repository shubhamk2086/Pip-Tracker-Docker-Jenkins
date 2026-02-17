 pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/shubhamk2086/Pip-Tracker-Docker-Jenkins.git'
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                sh 'docker build -t your-dockerhub-username/pip-tracker-frontend:$BUILD_NUMBER ./frontend'
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                sh 'docker build -t your-dockerhub-username/pip-tracker-backend:$BUILD_NUMBER ./backend'
            }
        }

        stage('Push Docker Images') {
            steps {
                sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push your-dockerhub-username/pip-tracker-frontend:$BUILD_NUMBER
                    docker push your-dockerhub-username/pip-tracker-backend:$BUILD_NUMBER
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Frontend & Backend images built and pushed successfully!"
        }
        failure {
            echo "❌ Pipeline failed!"
        }
    }
}
