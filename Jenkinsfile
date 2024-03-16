pipeline {
    agent any
    environment{
        registry = "rpdharanidhar/devops-task01"
        DOCKER_IMAGE = "rpdharanidhar/devops-task01:latest"
        KUBE_NAMESPACE = "jenkinsdemo-kube"
        DOCKER_PASSWORD = credentials('docker-password')
        DOCKER_USERNAME = credentials('docker-username')
        DOCKER_IMAGE_NAME = "rpdharanidhar/devops-task01"
        DOCKER_HUB_REPO = "rpdharanidhar"
        DOCKER_REGISTRY = "rpdharanidhar/devops-task01"
    }
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/rpdharanidhar/devops-task01.git', branch: 'main', credentialsId: 'git-credentials'
            }
        }
        stage('Build Docker Image') {
            steps {
                bat "docker build -t rpdharanidhar/devops-task01:latest ."
            }
        }
        stage('Push Docker Image to Hub') {
            steps {
                bat "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD} && docker push rpdharanidhar/devops-task01:latest"
            }
        
        }
        stage('Run Docker Container') {
            steps {
                bat "docker run -d -p 8080:80 ${DOCKER_IMAGE}"
            }
        }
        // stage('Deploy to Kubernetes') {
        //     steps {
        //         bat 'cd /d D:/DevOps/kube/training/kubetst/demo'
        //         bat 'kubectl apply -f web-deployment.yaml --validate=false'
        //         bat 'kubectl port-forward deployment/nginx-deployment 8080:80'
        //         bat 'kubectl get pods'
        //     }
        // }
        stage('Cleaning up') {
            steps{
                bat "docker rmi $registry:$BUILD_NUMBER"
            }
        }
    }
}
