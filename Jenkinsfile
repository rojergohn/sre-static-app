// Jenkinsfile for CI/CD pipeline to build and push Docker image to AWS ECR
// Need to edit, replace YOUR_ACCOUNT_ID and YOUR_GITHUB_USERNAME with actual values before running the pipeline

pipeline {

    agent any

    environment {
        AWS_ACCOUNT_ID = 'YOUR_ACCOUNT_ID'
        REGION = 'ap-south-1'
        REPO_NAME = '205994119856.dkr.ecr.us-east-1.amazonaws.com/srestaticappimg' // corrected Done
    }

    stages {

        stage('Clone Repo') {
            steps {
                git 'https://github.com/YOUR_GITHUB_USERNAME/sre-static-app.git'
            }
        }

        stage('Build Artifact') {
            steps {
                sh 'chmod +x build.sh'
                sh './build.sh'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t sre-static-app .'
            }
        }

        stage('Login to AWS ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $REGION \
                | docker login --username AWS \
                --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com
                '''
            }
        }

        stage('Tag Image') {
            steps {
                sh '''
                docker tag sre-static-app:latest \
                $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/$REPO_NAME:latest
                '''
            }
        }

        stage('Push Image') {
            steps {
                sh '''
                docker push \
                $AWS_ACCOUNT_ID.dkr.ecr.$REGION.amazonaws.com/$REPO_NAME:latest
                '''
            }
        }
    }
}