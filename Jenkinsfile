pipeline {
    agent {
        docker { image 'node:18' }
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/pawankumarreddy1/nodejs-project.git'
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

    post {
        success {
            echo "Node.js app running in Docker container locally on port 3000 🚀"
        }
        failure {
            echo "Build or deployment failed."
        }
    }
}

