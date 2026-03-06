pipeline {
    agent any
    stages {
        stage('AWS Login') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-creds-sudhanshu']]) {
                    sh 'aws ecr get-login-password --region us-west-1 | docker login --username AWS --password-stdin 975050024946.dkr.ecr.us-west-1.amazonaws.com'
                }
            }
        }
        stage('Frontend') {
            steps { dir('frontend') { script { def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-frontend:${BUILD_NUMBER}"); image.push(); image.push('latest') } } }
        }
        stage('Auth Service') { steps { dir('backend/authService') { script { def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-auth:${BUILD_NUMBER}"); image.push(); image.push('latest') } } } }
        stage('Admin Service') { steps { dir('backend/adminService') { script { def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-admin:${BUILD_NUMBER}"); image.push(); image.push('latest') } } } }
        stage('Chat Service') { steps { dir('backend/chatService') { script { def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-chat:${BUILD_NUMBER}"); image.push(); image.push('latest') } } } }
        stage('Streaming Service') { steps { dir('backend/streamingService') { script { def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-streaming:${BUILD_NUMBER}"); image.push(); image.push('latest') } } } }
    }
    post { always { sh 'docker logout 975050024946.dkr.ecr.us-west-1.amazonaws.com || true' } }
}
