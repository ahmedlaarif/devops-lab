pipeline {
    agent any
    environment {
        IMAGE_NAME = "webapp"
        IMAGE_TAG = "latest"
    }
    stages {
        stage('Build Docker Image') {
            steps {
                bat "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }
        stage('Load Image to Minikube') {
            steps {
                bat "minikube image load ${IMAGE_NAME}:${IMAGE_TAG}"
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                bat "kubectl apply -f deployment.yaml"
                bat "kubectl rollout restart deployment/${IMAGE_NAME}"
            }
        }
    }
}
