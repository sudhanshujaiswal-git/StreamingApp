pipeline {
    agent any
    stages {
        stage('AWS Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-creds-sudhanshu', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh '''
                    export AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID
                    export AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY
                    aws ecr get-login-password --region us-west-1 | docker login --username AWS --password-stdin 975050024946.dkr.ecr.us-west-1.amazonaws.com
                    echo "✅ Docker login successful"
                    '''
                }
            }
        }
        stage('Frontend') {
            steps { 
                dir('frontend') { 
                    script { 
                        def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-frontend:${BUILD_NUMBER}")
                        image.push()
                        image.push('latest')
                        echo "✅ Frontend pushed"
                    } 
                } 
            }
        }
        stage('Auth Service') { 
            steps { 
                dir('backend/authService') { 
                    script { 
                        def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-auth:${BUILD_NUMBER}")
                        image.push()
                        image.push('latest')
                        echo "✅ Auth service pushed"
                    } 
                } 
            } 
        }
        stage('Admin Service') { 
            steps { 
                dir('backend/adminService') { 
                    script { 
                        def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-admin:${BUILD_NUMBER}")
                        image.push()
                        image.push('latest')
                        echo "✅ Admin service pushed"
                    } 
                } 
            } 
        }
        stage('Chat Service') { 
            steps { 
                dir('backend/chatService') { 
                    script { 
                        def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-chat:${BUILD_NUMBER}")
                        image.push()
                        image.push('latest')
                        echo "✅ Chat service pushed"
                    } 
                } 
            } 
        }
        stage('Streaming Service') { 
            steps { 
                dir('backend/streamingService') { 
                    script { 
                        def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-streaming:${BUILD_NUMBER}")
                        image.push()
                        image.push('latest')
                        echo "✅ Streaming service pushed"
                    } 
                } 
            } 
        }
    }
    post { 
        always { 
            sh 'docker logout 975050024946.dkr.ecr.us-west-1.amazonaws.com || true'
            echo "🏁 Pipeline completed!"
        } 
    }
}
