pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "mahammadsahiltricon/python-demo"   // replace with your Docker Hub username
    }

    stages {
        stage('Checkout') {
            steps {
                echo "✅ Checking out code..."
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "✅ Installing dependencies..."
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Docker Build & Push') {
            environment {
                DOCKERHUB_CREDENTIALS = credentials('docker-hub-credentials') // use your Jenkins credentials ID
            }
            steps {
                echo "🐳 Building Docker image..."
                sh "docker build -t ${DOCKER_IMAGE}:latest ."

                echo "🔑 Logging in to Docker Hub..."
                sh "echo ${DOCKERHUB_CREDENTIALS_PSW} | docker login -u ${DOCKERHUB_CREDENTIALS_USR} --password-stdin"

                echo "📤 Pushing image to Docker Hub..."
                sh "docker push ${DOCKER_IMAGE}:latest"
            }
        }
    }

    post {
        success {
            echo "✅ Build and push completed successfully!"
        }
        failure {
            echo "❌ Build or push failed!"
        }
    }
}
