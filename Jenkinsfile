pipeline {
    agent any
    
    environment {
        DOCKER_CREDENTIALS = credentials('docker-hub-credentials-id')
        DOCKER_IMAGE_NAME = "testdemo-jenkins"
        DOCKER_HUB_REPO = "rpdharanidhar/devops-integration"
        DOCKER_USERNAME = "rpdharanidhar"
        DOCKER_PASSWORD = "Dharanirp@1482"
        
    }
    
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/rpdharanidhar/devops-automation.git', branch: 'main', credentialsId: 'polar-git-credentials'
            }
        }
        stage('Docker Login') {
            steps {
                script {
                    docker.withRegistry('docker pull rpdharanidhar/devops-integration:latest', "${docker-hub-credentials-id}") {
                        // This block is authenticated with Docker Hub.
                        // You can now pull/push images to Docker Hub.
                        // withCredentials([usernamePassword(credentialsId: env.DOCKER_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                            // docker.withRegistry(env.DOCKER_REGISTRY, env.DOCKER_USERNAME, env.DOCKER_PASSWORD)
                        withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials-id', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                            sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
                        
                        }
                    }
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script { 
                     //sh 'docker build -t testdemo-jenkins https://github.com/rpdharanidhar/devops-automation.git#rpdharanidhar/devops-integration', branch: 'main', credentialsId: 'polar-git-credentials'
                    docker.build("${DOCKER_IMAGE_NAME}")
                }
            }
        }
        // stage('Push image to Hub'){
        //     steps{
        //         script{
        //             withCredentials([string(credentialsId: 'docker-hub-credentials-id', variable: 'docker-hub-credentials-id')]) {
        //             sh 'docker tag testdemo-jenkins rpdharanidhar/devops-integration:latest'
        //             sh 'docker login -u rpdharanidhar -p ${docker-hub-credentials-id}'
        //             sh 'docker push rpdharanidhar/devops-integration:latest'
        //             }
        //         }
        //     }
        // }
        stage('Push Docker Image to Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${docker-hub-credentials-id}") {
                        docker.image("${DOCKER_IMAGE_NAME}").push("${env.DOCKER_HUB_REPO}:${env.BUILD_NUMBER}")
                        docker.image("${DOCKER_IMAGE_NAME}").push("latest")
                    }
                }
            }
        }
        // stage('Run Docker Container') {
        //     steps {
        //         script {
        //             //docker.image("testdemo-jenkins").run("-p 8080:8080")
        //             sh 'docker run --name "${DOCKER_IMAGE_NAME}" -p 8080:80 -d nginx'
        //         }
        //     }
        // }
        stage('Run Docker Container') {
            steps {
                script {
                    // Run a Docker container from the built image
                    def dockerContainer = docker.image("${DOCKER_IMAGE_NAME}").run('-d')
                    // Get the container ID
                    def containerId = dockerContainer.id
                    // Print the container ID
                    println "Docker container ID: ${containerId}"
                }
            }
        }
    }
}
