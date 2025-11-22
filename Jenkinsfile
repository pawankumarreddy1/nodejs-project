pipeline {
    agent any

    environment {
        IMAGE_NAME = "nodejs-project-local"
        IMAGE_TAG  = "latest"
        PORT = "3000"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/pawankumarreddy1/nodejs-project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME:$IMAGE_TAG ."
            }
        }

        stage('Run Docker Container') {
            steps {
                // Stop previous container if exists
                sh """
                if [ \$(docker ps -q -f name=$IMAGE_NAME) ]; then
                    docker stop $IMAGE_NAME
                    docker rm $IMAGE_NAME
                fi
                """

                // Run the container
                sh "docker run -d -p $PORT:$PORT --name $IMAGE_NAME $IMAGE_NAME:$IMAGE_TAG"
            }
        }
    }

    post {
        success {
            echo "Node.js app is running in a local Docker container on port $PORT 🚀"
        }
        failure {
            echo "Build or deployment failed. Check logs."
        }
    }
}

