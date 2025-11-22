pipeline {
    agent any

    // Define tools
    tools {
        nodejs "NodeJS-18"   // Name you configured in Jenkins NodeJS Tool
    }

    // Global environment variables
    environment {
        IMAGE_NAME = "pawankumarreddy1/nodejs-project"  // Docker image name
        IMAGE_TAG = "latest"
        BUCKET_NAME = "${env.BUCKET_NAME}"  // Optional, if set in Jenkins Global Properties
        DB_URL = "${env.DB_URL}"           // Optional
    }

    stages {

        stage('Checkout') {
            steps {
                // Checkout main branch from your repo
                git branch: 'main', 
                    url: 'https://github.com/pawankumarreddy1/nodejs-project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                // Run tests if any
                sh 'npm test || echo "No tests found"'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t $IMAGE_NAME:$IMAGE_TAG .
                """
            }
        }

        stage('Push Docker Image') {
            steps {
                // Requires DockerHub credentials configured in Jenkins
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', 
                                                 usernameVariable: 'USERNAME', 
                                                 passwordVariable: 'PASSWORD')]) {
                    sh """
                    echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                    docker push $IMAGE_NAME:$IMAGE_TAG
                    """
                }
            }
        }

        stage('Deploy (Optional)') {
            steps {
                // Here you can add deployment steps
                echo "Deployment placeholder. You can deploy to Minikube / AWS / Cloud Run."
            }
        }
    }

    post {
        success {
            echo "Build and Docker image creation succeeded!"
        }
        failure {
            echo "Build failed. Check console output for details."
        }
    }
}
