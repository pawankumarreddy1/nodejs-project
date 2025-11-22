pipeline {
    agent any

    tools {
        nodejs "NodeJS-18"  // Name you configured in Jenkins
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/pawankumarreddy1/nodejs-project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t nodejs-project-local:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker stop nodejs-project-local || true
                docker rm nodejs-project-local || true
                docker run -d -p 3000:3000 --name nodejs-project-local nodejs-project-local:latest
                '''
            }
        }
    }
}
