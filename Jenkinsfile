pipeline {
    tools {
        maven 'Maven3'
    }
    agent any

    environment {
        registry = "654654352254.dkr.ecr.us-east-1.amazonaws.com/my-eks-image-repo"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Shubhre97/devopscoach-springboot-proj.git']])
            }
        }
        
        stage ("Build JAR") {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage ("Build Image") {
            steps {
                script {
                    docker.build registry
                    dockerImage.tag("$BUILD_NUMBER")
                }
            }
        }
        
        stage ("Push to ECR") {
            steps {
                script {
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 654654352254.dkr.ecr.us-east-1.amazonaws.com"
                    sh "docker push 654654352254.dkr.ecr.us-east-1.amazonaws.com/my-eks-image-repo:$BUILD_NUMBERtest"
                    
                }
            }
        }
        
        stage ("Helm Deploy") {
            steps {
                    sh "helm upgrade first --install mychart --namespace helm-deployment --set image.tag=$BUILD_NUMBER"
                }
            }
        }
}

