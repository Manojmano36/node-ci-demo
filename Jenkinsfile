pipeline {
    agent { label 'agent-jen' }

    environment {
        AWS_REGION = "us-east-1"
        REPO_URI = "public.ecr.aws/g3a2j9b7/jenkins/mano-jenkins"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Build App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t node-ci-demo:${BUILD_NUMBER} .
                docker tag node-ci-demo:${BUILD_NUMBER} ${REPO_URI}:${BUILD_NUMBER}
                """
            }
        }

        stage('Login to ECR') {
            steps {
                withAWS(credentials: 'aws-ecr-creds', region: "${AWS_REGION}") {
                    sh """
                    aws ecr get-login-password --region ${AWS_REGION} \
                    | docker login --username AWS --password-stdin ${REPO_URI}
                    """
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh """
                docker push ${REPO_URI}:${BUILD_NUMBER}
                """
            }
        }
    }

    post {
        always {
            echo "Pipeline finished"
        }
    }
}
