pipeline {
    agent any

   tools {
        maven 'Maven3'
    }
    
    environment {
        registry = "010526274628.dkr.ecr.ap-southeast-2.amazonaws.com/spring-service"
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
                    dockerImage = docker.build registry 
                    dockerImage.tag("$BUILD_NUMBER")
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region ap-southeast-2 | docker login --username AWS --password-stdin 010526274628.dkr.ecr.ap-southeast-2.amazonaws.com"
                    sh "docker push 010526274628.dkr.ecr.ap-southeast-2.amazonaws.com/spring-service:$BUILD_NUMBER"
                    
                }
            }
        }
                
        stage ("Helm install") {
            steps {
                    sh "helm upgrade first --install feb24-chart --namespace helm-deployment --set image.tag=$BUILD_NUMBER"
                }
            }
    }
}
