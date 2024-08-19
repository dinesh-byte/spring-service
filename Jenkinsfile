pipeline {
    agent any

   tools {
        maven 'Maven3'
    }
    environment {
        ECR_REPO = '010526269830.dkr.ecr.ap-south-1.amazonaws.com/myspringrepo'
        AWS_REGION = 'ap-south-1'
        IMAGE_NAME = "${ECR_REPO}"
    }
    stages {
        
        stage ("Build JAR") {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    def imageTag = "${env.BUILD_NUMBER}"
                    docker.build("${IMAGE_NAME}:${imageTag}")
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 010526269830.dkr.ecr.ap-south-1.amazonaws.com"
                    def imageTag = "${env.BUILD_NUMBER}"
                    sh "docker push ${IMAGE_NAME}:${imageTag}"
                    
                }
            }
        }
                
        stage ("Helm install") {
            steps {
                script {
                    def imageTag = "${env.BUILD_NUMBER}"
                    sh """
                    helm upgrade --install my-release mychart \
                    --set image.repository=${IMAGE_NAME} \
                    --set image.tag=${imageTag} \
                    --namespace helm-deployment \
                    --recreate-pods
                    """
                }
            }
        }
    }
}
