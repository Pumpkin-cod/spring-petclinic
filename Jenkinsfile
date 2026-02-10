pipeline {
    agent any

    environment {
        IMAGE_NAME = "petclinic"
        IMAGE_TAG  = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run Tests (Dockerized Maven)') {
            steps {
                sh '''
                docker run --rm \
                  -v "$PWD":/app \
                  -w /app \
                  maven:3.9.6-eclipse-temurin-17 \
                  mvn clean test
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
                }
            }
        }
    }

    post {
        success {
            echo "Docker image ${IMAGE_NAME}:${IMAGE_TAG} built successfully 🚀"
        }
        failure {
            echo "Pipeline failed ❌"
        }
        always {
            sh 'docker ps -a'
        }
    }
}
