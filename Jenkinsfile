pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub')
        REPO = 'makwanji/web-app:v0.1.0'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/makwanji/jenkins.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $REPO:latest .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                sh 'echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker push $REPO:latest'
            }
        }
    }

    post {
        success {
            echo "Docker image pushed successfully to Docker Hub."
        }
        failure {
            echo "Build failed. Check the logs."
        }
    }
}
