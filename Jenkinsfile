pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="890936820974"
        AWS_DEFAULT_REGION="us-east-1"
        IMAGE_REPO_NAME="jenkins-ecr"
        IMAGE_TAG="latest"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
    stages {
        stage('Logging into AWS ECR') {
            steps {
                script {
                    sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 890936820974.dkr.ecr.us-east-1.amazonaws.com"
                }
            }
        }
        stage('Cloning Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/sd031/aws_codebuild_codedeploy_nodeJs_demo.git']]])
            }
        }
        // Building Docker images
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build "${jenkins-docker-hub}:${latest}"
                }
            }
        }
        // Uploading Docker images into AWS ECR
        stage('Pushing to ECR') {
            steps {
                script {
                    sh "docker tag jenkins-ecr:latest 890936820974.dkr.ecr.us-east-1.amazonaws.com/jenkins-ecr:latest"
                    sh "docker push 890936820974.dkr.ecr.us-east-1.amazonaws.com/jenkins-ecr:latest"

                }
            }
        }
    }
}
