pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub_token') // Jenkins secret ID
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
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS) {
                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl config use-context terraform-user-new@trend-cluster.us-east-1.ek

