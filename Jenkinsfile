pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS =dckr_pat_rD44YYUQrIk8SQLdYH1IDKtebWA
        IMAGE_NAME = 'harshaa1424/mini-project-1'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Harshaa1424/Mini-Project-1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl config use-context terraform-user-new@trend-cluster.us-east-1.eksctl.io
                kubectl set image deployment/trend-app-deployment  trend-app=${IMAGE_NAME}:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'Build, push, and deploy successful!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
