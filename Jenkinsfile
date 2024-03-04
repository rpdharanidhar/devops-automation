pipeline {
    agent any
   
    // environment {
    //     DOCKER_CREDENTIALS = credentials('dockerhub')
    //     DOCKER_IMAGE_NAME = "rpdharanidhar/devops-integration"
    //     DOCKER_HUB_REPO = "rpdharanidhar"
    // }
    environment {
        // Define the Azure VM details
        AZURE_VM_IP = '172.208.57.242'
        AZURE_VM_USERNAME = 'admin'
        AZURE_VM_PRIVATE_KEY = credentials('azure-vm')
    }
   
    stages {
        stage('Connect to Azure VM') {
            steps {
                script {
                    // Start the SSH agent and add the private key
                    // sshagent(['azure-vm']) {
                        // Use SSH to connect to the Azure VM and execute a command
                        // sshCommand remote: "ubuntu@${AZURE_VM_IP}", command: 'echo Hello from Jenkins'
                        sh 'scp -r *  azureadmin@172.208.57.242:/var/www/html/'
                    }
                }
            }
        

        stage('Checkout') {
            steps {
                git url: 'https://github.com/rpdharanidhar/devops-automation.git', branch: 'main', credentialsId: 'git-credentials'
            }
        }
        // stage('Docker Login') {
        //     steps {
        //         script {
        //             docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS}") {
        //                 // This block is authenticated with Docker Hub.
        //                 // You can now pull/push images to Docker Hub.
        //                 // withCredentials([usernamePassword(credentialsId: env.DOCKER_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
        //                     // docker.withRegistry(env.DOCKER_REGISTRY, env.DOCKER_USERNAME, env.DOCKER_PASSWORD)
        //                 withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS}", usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
        //                     sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD}"
        //                 // sh "docker login"
        //                 }
        //             }
        //         }
        //     }
        // }
        stage('Build Docker Image') {
            steps {
                script {
                     //sh 'docker build -t testdemo-jenkins https://github.com/rpdharanidhar/devops-automation.git#rpdharanidhar/devops-integration', branch: 'main', credentialsId: 'polar-git-credentials'
                    docker.build("rpdharanidhar/devops-integration:latest")
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
                    sh 'docker tag testdemo-jenkins rpdharanidhar/devops-integration:latest'
                    sh 'docker login -u rpdharanidhar -p ${docker-pass-txt}'
                    sh 'docker push rpdharanidhar/devops-integration:latest'
                    }
                }
            }
        }
        // stage('Push Docker Image to Hub') {
        //     steps {
        //         script {
        //             docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS}") {
        //                // docker.image("${DOCKER_IMAGE_NAME}").push("${env.DOCKER_HUB_REPO}:${env.BUILD_NUMBER}")
        //                 docker.image("${DOCKER_IMAGE_NAME}:latest").push()
        //             }
        //         }
        //     }
        // }
        // stage('Run Docker Container') {
        //     steps {
        //         script {
        //             //docker.image("testdemo-jenkins").run("-p 8080:8080")
        //             sh 'docker run --name "${DOCKER_IMAGE_NAME}" -p 8080:80 -d nginx'
        //         }
        //     }
        // }
        // // stage('Run Docker Container') {
        // //     steps {
        // //         script {
        // //             // Run a Docker container from the built image
        // //             def dockerContainer = docker.image("${DOCKER_IMAGE_NAME}").run('-d')
        // //             // Get the container ID
        // //             def containerId = dockerContainer.id
        // //             // Print the container ID
        // //             println "Docker container ID: ${containerId}"
        // //         }
        // //     }
        // // }
    }
    // post{
    //     failure {
    //         mailto: 'dharanirp2002@gmail.com', subject: 'The Pipeline failed :('
    //     }
    // }
}