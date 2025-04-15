pipeline {
    agent any

    environment {
        IMAGE_NAME = "pratikp02/simple-webapp"
        TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/pratikp02/simple-webapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${TAG}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withDockerRegistry(credentialsId: 'dockerhub-creds', url: '') {
                    script {
                        docker.image("${IMAGE_NAME}:${TAG}").push()
                    }
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh "docker stop webapp || true"
                    sh "docker rm webapp || true"
                    sh "docker run -d --name webapp -p 5000:5000 ${IMAGE_NAME}:${TAG}"
                }
            }
        }
    }
}
