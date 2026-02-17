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

