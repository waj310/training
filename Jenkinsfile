pipeline {
  agent any
  
  environment {
    IMAGE_NAME = "node-docker-jenkins-app"
    AWS_REGION = "us-east-2"
    ECR_REPO = '640168453782.dkr.ecr.us-east-2.amazonaws.com/argocd'
    TAG = "latest"
    AWS_ACCESS_KEY_ID     = "AKIAZKDIDM2LNNRRDR6S"
    AWS_SECRET_ACCESS_KEY = "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" 
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
        bat "docker build -t ${ECR_REPO}:${TAG} ."
      }
    }

   stage('Login to ECR') {
      steps {
        bat "aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $ECR_REPO"
      }
    }

    stage('Push to ECR') {
      steps {
        bat "docker push ${ECR_REPO}:${TAG}"
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
