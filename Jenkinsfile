pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') // Jenkins Credential ID
        DOCKER_IMAGE = 'your-dockerhub-username/laravel-app'          // Replace with your DockerHub Username
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker Image..."
                    sh "docker build -t ${DOCKER_IMAGE}:${BUILD_NUMBER} ."
                    sh "docker tag ${DOCKER_IMAGE}:${BUILD_NUMBER} ${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Login & Push to DockerHub') {
            steps {
                script {
                    echo "Pushing image to DockerHub..."
                    sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                    sh "docker push ${DOCKER_IMAGE}:${BUILD_NUMBER}"
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    echo "Deploying container on EC2..."
                    sh "docker stop laravel-app-container || true"
                    sh "docker rm laravel-app-container || true"
                    sh "docker run -d --name laravel-app-container -p 8000:80 ${DOCKER_IMAGE}:latest"
                }
            }
        }
    }

    post {
        always {
            sh "docker logout"
        }
    }
}
