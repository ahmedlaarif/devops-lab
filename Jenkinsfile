pipeline {
    agent any

    environment {
        // Paths for Docker and Minikube on your Windows machine
        PATH = "C:\\Program Files\\Docker\\Docker\\resources\\bin;C:\\Program Files\\Kubernetes\\Minikube;${env.PATH}"
        
        // This tells Jenkins where to find the .minikube directory for certificates
        MINIKUBE_HOME = "C:\\Users\\ahmed"
        
        IMAGE_NAME = "webapp"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Environment Check') {
            steps {
                bat "docker --version"
                bat "minikube version"
                bat "kubectl version --client"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the image locally
                    bat "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Load Image to Minikube') {
            steps {
                script {
                    // Load the image into Minikube (now with the correct HOME path)
                    bat "minikube image load ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply manifests and restart deployment
                    bat "kubectl apply -f deployment.yaml"
                    bat "kubectl apply -f service.yaml"
                    bat "kubectl rollout restart deployment/${IMAGE_NAME}"
                }
            }
        }
    }

    post {
        success {
            echo 'SUCCESS: 100% Complete! The pipeline is fully functional.'
        }
        failure {
            echo 'FAILURE: Still failing? Check the "Load Image" stage console logs.'
        }
    }
}