pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub')
        REPO = 'makwanji'
        IMAGE_SVCA = 'we-app-service-a'
        IMAGE_SVCB = 'we-app-service-b'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'demo1', url: 'https://github.com/makwanji/jenkins.git'
            }
        }
        stage('Build Docker Image') {
            // steps {
            //     sh 'docker build -t $REPO:latest .'
            // }

            parallel {
                stage('Service A') {
                    steps {
                        sh 'cd service-a && docker build -t $REPO/$IMAGE_SVCA:latest'
                    }
                }
                stage('Service B') {
                    steps {
                        sh 'cd service-b && docker build -t $REPO/$IMAGE_SVCB:latest'
                    }
                }
            }
        }
        stage('Login to Docker Hub') {
            steps {
                sh 'echo $DOCKER_CREDENTIALS_PSW | docker login -u $DOCKER_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push Docker Image') {
            // steps {
            //     sh 'docker push $REPO:latest'
            // }


            parallel {
                stage('Service A') {
                    steps {
                        sh 'docker push $REPO/$IMAGE_SVCA:latest'
                    }
                }
                stage('Service B') {
                    steps {
                        sh 'docker push $REPO/$IMAGE_SVCB:latest'
                    }
                }
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
