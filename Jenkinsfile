pipeline {
    agent any
    environment {
        AWS_ACCOUNT = '975050024946'
        AWS_REGION = 'us-west-1'
        ECR_FRONTEND = "${AWS_ACCOUNT}.dkr.ecr.${AWS_REGION}.amazonaws.com/streamingapp-frontend"
        ECR_AUTH = "${AWS_ACCOUNT}.dkr.ecr.${AWS_REGION}.amazonaws.com/streamingapp-auth"
        ECR_STREAMING = "${AWS_ACCOUNT}.dkr.ecr.${AWS_REGION}.amazonaws.com/streamingapp-streaming"
        ECR_ADMIN = "${AWS_ACCOUNT}.dkr.ecr.${AWS_REGION}.amazonaws.com/streamingapp-admin"
        ECR_CHAT = "${AWS_ACCOUNT}.dkr.ecr.${AWS_REGION}.amazonaws.com/streamingapp-chat"
    }
    stages {
        stage('Build Images') {
            parallel {
                stage('Frontend') { steps { sh 'docker build -t frontend ./frontend' } }
                stage('Auth') { steps { sh 'docker build -t auth ./backend/authService' } }
                stage('Streaming') { steps { sh 'docker build -t streaming ./backend/streamingService' } }
                stage('Admin') { steps { sh 'docker build -t admin ./backend/adminService' } }
                stage('Chat') { steps { sh 'docker build -t chat ./backend/chatService' } }
            }
        }
    }
}
