pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "your_dockerhub_username/python-demo"
    }

    stages {
        stage('Checkout') {
            steps {
                echo '✅ Checking out code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo '✅ Installing dependencies...'
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --break-system-packages -r requirements.txt
                '''
            }
        }

        stage('Docker Build & Push') {
            steps {
                echo '🐳 Building and pushing Docker image...'
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker build -t $DOCKER_IMAGE .
                    docker push $DOCKER_IMAGE
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build and push successful!'
        }
        failure {
            echo '❌ Build or push failed!'
        }
    }
}
