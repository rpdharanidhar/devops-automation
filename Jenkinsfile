pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/varshatraining/demo.git', branch: 'main', credentialsId: 'git-credential'
            }
        }

        stage('Copy to Nginx root') {
            steps {
                // Copying webcontent in to remote server
                sh 'scp -r *  azureadmin@13.95.151.203:/var/www/html/'
            }
        }
    }
}
