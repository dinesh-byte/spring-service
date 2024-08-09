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
                    docker.build registry
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region ap-southeast-2 | docker login --username AWS --password-stdin 010526274628.dkr.ecr.ap-southeast-2.amazonaws.com"
                    sh "docker push 010526274628.dkr.ecr.ap-southeast-2.amazonaws.com/spring-service:latest"
                    
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
