pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Local Docker Image') {
            steps {
                script {
                    echo "Building Docker Image locally..."
                    sh "docker build -t laravel-app:latest ."
                }
            }
        }

        stage('Deploy Locally') {
            steps {
                script {
                    echo "Deploying container locally..."
                    sh "docker stop laravel-app-container || true"
                    sh "docker rm laravel-app-container || true"
                    sh "docker run -d --name laravel-app-container -p 8000:80 laravel-app:latest"
                }
            }
        }
    }
}
