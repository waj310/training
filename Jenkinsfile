pipeline {
  agent any
  
  environment {
    IMAGE_NAME = "node-docker-jenkins-app"
    TAG = "latest"
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }

    stage('Build Docker Image') {
      steps {
        bat "docker build -t ${IMAGE_NAME}:${TAG} ."
      }
    }

    stage('Save Docker Image') {
      steps {
        bat "docker save ${IMAGE_NAME}:${TAG} -o ${IMAGE_NAME}.tar"
      }
    }
  }

  post {
    success {
      archiveArtifacts artifacts: '*.tar', fingerprint: true
      echo "✅ Docker image saved and archived as ${IMAGE_NAME}.tar"
    }
    failure {
      echo "❌ Build failed"
    }
  }
}
