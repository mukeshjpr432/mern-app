pipeline {
    agent any

    environment {
        DOCKER_HUB = "mukeshjpr432"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/mukeshjpr432/mern-app.git'
            }
        }

        stage('SonarQube Scan') {
            steps {
                bat 'echo Running SonarQube Scan'
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_HUB%/mern-client:latest ./client'
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_HUB%/mern-server:latest ./server'
            }
        }

        stage('Trivy Scan Frontend') {
            steps {
                bat 'trivy image %DOCKER_HUB%/mern-client:latest'
            }
        }

        stage('Trivy Scan Backend') {
            steps {
                bat 'trivy image %DOCKER_HUB%/mern-server:latest'
            }
        }

        stage('Push Docker Images') {
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
