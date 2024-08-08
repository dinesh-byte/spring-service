pipeline {
    agent any

   tools {
        maven 'Maven3'
    }
    
    environment {
        registry = "010526269830.dkr.ecr.us-east-1.amazonaws.com/micro-spring:latest"
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
                    docker.build registry
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 010526269830.dkr.ecr.us-east-1.amazonaws.com"
                    sh "docker push 010526269830.dkr.ecr.us-east-1.amazonaws.com/micro-spring:latest"
                    
                }
            }
        }
        
        stage ("Helm package") {
            steps {
                    sh "helm package springboot"
                }
            }
                
        stage ("Helm install") {
            steps {
                    sh "helm upgrade myrelease-21 springboot-0.1.0.tgz"
                }
            }
    }
}
