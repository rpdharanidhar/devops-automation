pipeline {
    environment {
    registry = "rpdharanidhar/web"
    registryCredential = 'dockerhub'
    }
    agent any

    stages {
        stage('Cloning the Git') {
            steps {
                git url: 'https://github.com/rpdharanidhar/demo.git', branch: 'main', credentialsId: 'git-credential'
            }
        }

        stage('Build the Docker images') {
            steps{
                script {
                    docker.build registry + ":$BUILD_NUMBER"
                }


            }
        }

        stage('Deploy the built Image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
