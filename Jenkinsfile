pipeline {
    agent any

    environment {
        DOCKER_HUB = "mukeshjpr432"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/mukeshjpr432/mern-app.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    echo "Running SonarQube Scan"
                }
            }
        }

        stage('Build Client Image') {
            steps {
                bat 'docker build -t %DOCKER_HUB%/mern-client:latest ./client'
            }
        }

        stage('Build Server Image') {
            steps {
                bat 'docker build -t %DOCKER_HUB%/mern-server:latest ./server'
            }
        }

        stage('Trivy Client Scan') {
            steps {
                bat 'trivy image %DOCKER_HUB%/mern-client:latest'
            }
        }

        stage('Trivy Server Scan') {
            steps {
                bat 'trivy image %DOCKER_HUB%/mern-server:latest'
            }
        }

        stage('Push Images') {
            steps {
                bat 'docker push %DOCKER_HUB%/mern-client:latest'
                bat 'docker push %DOCKER_HUB%/mern-server:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f k8s/'
            }
        }
    }
}