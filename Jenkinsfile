pipeline {
  # agent slave node
    agent any

    environment {
        GIT_CREDENTIALS_ID = 'github-creds' 
        GIT_REPO = 'https://github.com/waj310/training'
    }

    stages {
        stage('Checkout Repository') {
            steps {
                script {
                    if (isUnix()) {
                        error "This pipeline is intended for a Windows agent."
                    }
                }

                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']], 
                    userRemoteConfigs: [[
                        url: "${env.GIT_REPO}",
                        credentialsId: "${env.GIT_CREDENTIALS_ID}"
                    ]]
                ])
            }
        }

        stage('Run commands.bat') {
            steps {
                bat 'commands.bat'
            }
        }
    }

    post {
        success {
            echo ' Pipeline completed successfully!'
        }
        failure {
            echo ' Pipeline failed.'
        }
    }
}
