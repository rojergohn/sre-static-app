pipeline {

    agent any

    environment {
        AWS_REGION = 'us-east-1'
        AWS_ACCOUNT_ID = '205994119856'
        IMAGE_NAME = 'sre-static-app'
        ECR_REPO_NAME = 'srestaticappimg'
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
                sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .'
            }
        }

        stage('Push to ECR') {
            steps {
                withAWS(credentials: 'aws-jenkins-creds', region: "${AWS_REGION}") {

                    sh """
                    aws ecr get-login-password --region ${AWS_REGION} | \
                    docker login --username AWS \
                    --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                    """

                    sh """
                    docker tag ${IMAGE_NAME}:${BUILD_NUMBER} \
                    ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${BUILD_NUMBER}
                    """

                    sh """
                    docker push \
                    ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO_NAME}:${BUILD_NUMBER}
                    """
                }
            }
        }
    }
}