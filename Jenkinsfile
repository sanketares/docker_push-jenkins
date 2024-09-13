pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials' // Jenkins credential ID for Docker Hub
        DOCKER_IMAGE_NAME = 'sanket-docker'
        DOCKER_REGISTRY = 'docker.io' // Docker Hub registry URL
        IMAGE_TAG = 'latest' // Tag for your Docker image
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    docker.withRegistry("https://${DOCKER_REGISTRY}", "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}").push("${IMAGE_TAG}")
                    }
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE_NAME}:${IMAGE_TAG}").inside {
                        sh 'echo "Running container with the image"'
                        // Add any commands you want to run inside the container here
                        sh 'your-command-here'
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs() // Clean up workspace after the build
        }
    }
}
