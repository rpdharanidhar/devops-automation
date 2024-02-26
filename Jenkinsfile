pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/rpdharanidhar/devops-automation.git', branch: 'main', credentialsId: 'polar-git-credentials'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                     sh 'docker build https://github.com/rpdharanidhar/devops-automation.git#rpdharanidhar/devops-integration', branch: 'main', credentialsId: 'polar-git-credentials'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
                   sh 'docker login -u rpdharanidhar -p ${dockerhub}'
                   sh 'docker push rpdharanidhar/devops-integration:latest'
                    }
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    docker.image("devops-integration").run("-p 8080:8080")
                }
            }
        }
    }
}
