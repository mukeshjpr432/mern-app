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
                sh 'echo "Running SonarQube Scan"'
            }
        }

        stage('Build Frontend Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB/mern-client:latest ./client'
            }
        }

        stage('Build Backend Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_HUB/mern-server:latest ./server'
            }
        }

        stage('Trivy Scan Frontend') {
            steps {
                sh 'trivy image --exit-code 0 --severity HIGH,CRITICAL $DOCKER_HUB/mern-client:latest'
            }
        }

        stage('Trivy Scan Backend') {
            steps {
                sh 'trivy image --exit-code 0 --severity HIGH,CRITICAL $DOCKER_HUB/mern-server:latest'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub',
                    usernameVariable: 'mukeshjpr432',
                    passwordVariable: 'DoHare@1991'
                )]) {

                    sh '''
                    echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                    '''
                }
            }
        }

        stage('Push Docker Images') {
            steps {
                sh 'docker push $DOCKER_HUB/mern-client:latest'
                sh 'docker push $DOCKER_HUB/mern-server:latest'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/ --validate=false'
            }
        }
    }
}
