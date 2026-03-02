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
        stage('AWS Login & Docker Auth') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-creds']]) {
                    sh '''
                    aws ecr get-login-password --region us-west-1 | docker login --username AWS --password-stdin ${AWS_ACCOUNT}.dkr.ecr.us-west-1.amazonaws.com
                    echo "✅ Docker login successful"
                    '''
                }
            }
        }
        stage('Build Images') {
            parallel {
                stage('Frontend') { 
                    steps { 
                        dir('frontend') {
                            sh '''
                            docker build -t ${ECR_FRONTEND}:${BUILD_NUMBER} .
                            docker push ${ECR_FRONTEND}:${BUILD_NUMBER}
                            '''
                        }
                    } 
                }
                stage('Auth') { 
                    steps { 
                        dir('backend/authService') {
                            sh '''
                            docker build -t ${ECR_AUTH}:${BUILD_NUMBER} .
                            docker push ${ECR_AUTH}:${BUILD_NUMBER}
                            '''
                        }
                    } 
                }
                stage('Streaming') { 
                    steps { 
                        dir('backend/streamingService') {
                            sh '''
                            docker build -t ${ECR_STREAMING}:${BUILD_NUMBER} .
                            docker push ${ECR_STREAMING}:${BUILD_NUMBER}
                            '''
                        }
                    } 
                }
                stage('Admin') { 
                    steps { 
                        dir('backend/adminService') {
                            sh '''
                            docker build -t ${ECR_ADMIN}:${BUILD_NUMBER} .
                            docker push ${ECR_ADMIN}:${BUILD_NUMBER}
                            '''
                        }
                    } 
                }
                stage('Chat') { 
                    steps { 
                        dir('backend/chatService') {
                            sh '''
                            docker build -t ${ECR_CHAT}:${BUILD_NUMBER} .
                            docker push ${ECR_CHAT}:${BUILD_NUMBER}
                            '''
                        }
                    } 
                }
            }
        }
    }
}
