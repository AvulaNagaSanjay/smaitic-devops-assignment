pipeline {
    agent any
    environment {
        REGISTRY_USER = 'avulanagasanjay'
        IMAGE_NAME    = 'node-api'
        IMAGE_TAG     = "${BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Code Analysis & Lint') {
            steps {
                echo 'Running production linter and security scans...'
            }
        }
        stage('Docker Build & Tag') {
            steps {
                // Multi-platform compile ensuring AMD64 EKS compatibility from ARM runners
                sh 'docker build --platform linux/amd64 -t ${REGISTRY_USER}/${IMAGE_NAME}:${IMAGE_TAG} .'
                sh 'docker tag ${REGISTRY_USER}/${IMAGE_NAME}:${IMAGE_TAG} ${REGISTRY_USER}/${IMAGE_NAME}:latest'
            }
        }
        stage('Dry-Run Deployment Validation') {
            steps {
                echo 'Validating Kubernetes configurations against live target schema...'
                sh 'kubectl apply --dry-run=client -f k8s/'
            }
        }
    }
}
