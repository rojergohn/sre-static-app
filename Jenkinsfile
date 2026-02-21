// Jenkinsfile for CI/CD pipeline to build and push Docker image to AWS ECR
// Need to edit, replace YOUR_ACCOUNT_ID and YOUR_GITHUB_USERNAME with actual values before running the pipeline
pipeline {

    agent any

    environment {
        AWS_REGION = 'us-east-1'
        ECR_REPO = '205994119856.dkr.ecr.us-east-1.amazonaws.com/srestaticappimg'
        IMAGE_NAME = 'sre-static-app'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/rojergohn/sre-static-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    withAWS(credentials: 'aws-jenkins-creds', region: "${AWS_REGION}") {

                        def ecrLogin = ecrLogin()

                        sh ecrLogin

                        docker.withRegistry("https://${ECR_REPO}", "") {
                            docker.image("${IMAGE_NAME}:latest").push("latest")
                        }
                    }
                }
            }
        }
    }
}