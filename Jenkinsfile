pipeline {
    agent any
    environment{
        registry = "rpdharanidhar/devops-integration"
        DOCKER_IMAGE = "rpdharanidhar/devops-integration:latest"
        KUBE_NAMESPACE = "jenkinsdemo-kube"
        DOCKER_PASSWORD = credentials('docker-password')
        DOCKER_USERNAME = credentials('docker-username')
        DOCKER_IMAGE_NAME = "rpdharanidhar/devops-integration"
        DOCKER_HUB_REPO = "rpdharanidhar"
        DOCKER_REGISTRY = "rpdharanidhar/devops-integration"
    }
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/rpdharanidhar/devops-automation.git', branch: 'main', credentialsId: 'git-credentials'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Push Docker Image to Hub') {
            steps {
                bat "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD} && docker push rpdharanidhar/devops-integration"
            }
        
        }
        stage('Run Docker Container') {
            steps {
                bat "docker run -d ${DOCKER_IMAGE}"
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                bat 'cd /d D:/DevOps/kube/training/kubetst/demo'
                bat 'kubectl apply -f web-deployment.yaml'
                bat 'kubectl port-forward deployment/nginx-deployment 8080:80'
                bat 'kubectl get pods'
            }
        }
        stage('Cleaning up') {
            steps{
                bat "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}