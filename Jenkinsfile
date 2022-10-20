pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                // Build an image for scanning
                sh 'echo "FROM ubuntu:latest" > Dockerfile'
                sh 'docker build --no-cache -t ubuntu-image:0.1 .'
            }
        }
        stage('Download_WizCLI') {
            steps {
                // Download_WizCLI
                sh 'echo "Downloading wizcli..."'
                sh 'curl -o wizcli https://wizcli.app.wiz.io/wizcli'
                sh 'chmod +x wizcli'
            } 
        }
        stage('Auth_With_Wiz') {
            steps {
                // Auth with Wiz
                sh 'echo "Authenticating to the Wiz API..."'
                withCredentials([usernamePassword(credentialsId: 'wizcli', usernameVariable: 'ID', passwordVariable: 'SECRET')]) {
                sh './wizcli auth --id $ID --secret $SECRET'}
            }
        }
        stage('Scan') {
            steps {
                // Scanning the image
                sh 'echo "Scanning the image using wizcli..."'
                sh './wizcli docker scan --image ubuntu-image:0.1'
            }
        }
    }
}

