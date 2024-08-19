pipeline {
    agent any

   tools {
        maven 'Maven3'
    }
    environment {
        registry = "010526269830.dkr.ecr.ap-south-1.amazonaws.com/myspringrepo"
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
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 010526269830.dkr.ecr.ap-south-1.amazonaws.com"
                    sh "docker push 010526269830.dkr.ecr.ap-south-1.amazonaws.com/myspringrepo:latest"
                    
                }
            }
        }
                
        stage ("Helm install") {
            steps {
                    sh "helm upgrade first --install mychart --namespace helm-deployment"
                }
            }
    }
}
