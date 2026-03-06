pipeline {
    agent any
    
    stages {
        stage('AWS Login & Docker Auth') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-creds-sudhanshu']]) {
                    sh '''
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
                        image.push("latest")
                        echo "✅ Frontend pushed"
                    }
                }
            }
        }
        
        stage('Backend') {
            steps {
                dir('backend') {
                    script {
                        def image = docker.build("975050024946.dkr.ecr.us-west-1.amazonaws.com/streamingapp-backend:${BUILD_NUMBER}")
                        image.push()
                        image.push("latest")
                        echo "✅ Backend pushed"
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
