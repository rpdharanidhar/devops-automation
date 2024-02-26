pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/rpdharanidhar/devops-automation.git', branch: 'main', credentialsId: 'git-credential'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("testdemo1-jenkins")
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    docker.image("testdemo1-jenkins").run("-p 8080:8080")
                    }
                }
            }
        }
    }
}