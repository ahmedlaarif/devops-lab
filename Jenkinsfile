pipeline {
    agent any

    environment {
        // Paths for Docker and Minikube on your Windows machine
        PATH = "C:\\Program Files\\Docker\\Docker\\resources\\bin;C:\\Program Files\\Kubernetes\\Minikube;${env.PATH}"
        
        // This tells Jenkins where to find the .minikube directory for certificates
        MINIKUBE_HOME = "C:\\Users\\ahmed"

        // ADDED: This tells kubectl where to find the configuration file
        KUBECONFIG = "C:\\Users\\ahmed\\.kube\\config"
        
        IMAGE_NAME = "webapp"
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Environment Check') {
            steps {
                // Verify that the tools are accessible
                bat "docker --version"
                bat "minikube version"
                bat "kubectl version --client"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image using the Dockerfile in the root directory
                    bat "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Load Image to Minikube') {
            steps {
                script {
                    // Load the local image into the Minikube node environment
                    bat "minikube image load ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes manifests and force a restart to use the latest image
                    // Now with KUBECONFIG set, kubectl will know how to connect
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
            echo 'FAILURE: Still failing? Check the "Deploy to Kubernetes" stage console logs.'
        }
    }
}