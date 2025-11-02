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
        bat 'python --version'
        bat 'pip install -r requirements.txt'
      }
    }

    stage('Docker Build & Push') {
      steps {
        script {
          echo '🐳 Building Docker image...'
          bat "docker build -t %DOCKER_IMAGE% ."

          echo '🔐 Logging in to Docker Hub...'
          withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
            bat "docker login -u %DOCKER_USER% -p %DOCKER_PASS%"
          }

          echo '📦 Pushing image to Docker Hub...'
          bat "docker push %DOCKER_IMAGE%"
        }
      }
    }
  }

  post {
    success {
      echo '✅ Build and push completed successfully!'
    }
    failure {
      echo '❌ Build or push failed!'
    }
  }
}
