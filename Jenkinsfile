pipeline {
    agent any
    stages {
        stage('Build Frontend') {
            steps { 
                dir('frontend') { 
                    script { 
                        def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-frontend:${BUILD_NUMBER}")
                        echo "✅ Frontend built locally"
                    } 
                } 
            }
        }
        stage('Build Services') {
            parallel {
                stage('Auth') { 
                    steps { 
                        dir('backend/authService') { 
                            script { 
                                def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-auth:${BUILD_NUMBER}")
                                echo "✅ Auth built"
                            } 
                        } 
                    } 
                }
                stage('Admin') { 
                    steps { 
                        dir('backend/adminService') { 
                            script { 
                                def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-admin:${BUILD_NUMBER}")
                                echo "✅ Admin built"
                            } 
                        } 
                    } 
                }
                stage('Chat') { 
                    steps { 
                        dir('backend/chatService') { 
                            script { 
                                def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-chat:${BUILD_NUMBER}")
                                echo "✅ Chat built"
                            } 
                        } 
                    } 
                }
                stage('Streaming') { 
                    steps { 
                        dir('backend/streamingService') { 
                            script { 
                                def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-streaming:${BUILD_NUMBER}")
                                echo "✅ Streaming built"
                            } 
                        } 
                    } 
                }
            }
        }
    }
    post { 
        always { 
            echo "🏁 ALL SERVICES BUILT SUCCESSFULLY!"
        } 
    }
}
