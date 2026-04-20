pipeline {
    agent any

    environment {
        // Application image name and tag
        IMAGE_NAME = "webapp"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Checkout') {
            steps {
                // Fetch the source code from GitHub
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image locally
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Load Image to Minikube') {
            steps {
                script {
                    // Essential step to make the image visible to Minikube
                    sh "minikube image load ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes configurations
                    sh "kubectl apply -f deployment.yaml"
                    sh "kubectl apply -f service.yaml"
                    
                    // Force a rollout restart to pull the newly loaded image
                    sh "kubectl rollout restart deployment/${IMAGE_NAME}"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful! The application is running.'
        }
        failure {
            echo 'Deployment failed. Please check the logs for errors.'
        }
    }
}