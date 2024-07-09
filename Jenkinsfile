pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = 'python-web-app' // Your Docker Hub token credentials ID
        GIT_CREDENTIALS = 'python-web-app' // Your GitHub token credentials ID
        DOCKER_IMAGE = "rosinebelle/pyth:v.1.0"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/s7rosine/python-web-app.zip.git'
            }
        }
        
        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: "${DOCKER_HUB_CREDENTIALS}", variable: 'DOCKER_HUB_TOKEN')]) {
                        sh 'docker login -u rosinebelle -p "${DOCKER_HUB_TOKEN}"'
                    }
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${env.DOCKER_IMAGE}")
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
               sh 'docker push "${DOCKER_IMAGE}"'
            }
        }
       
    }
}