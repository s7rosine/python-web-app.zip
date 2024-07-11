pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = 'dockerhub-token' // Your Docker Hub token credentials ID
        GIT_CREDENTIALS = 'github-python-web-app' // Your GitHub token credentials ID
        DOCKER_IMAGE = "rosinebelle/pyth:v.1.0"
        K8S_CREDENTIALS = 'k8s-token-cred' // Your Kubernetes token credentials ID
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
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    withCredentials([string(credentialsId: "${K8S_CREDENTIALS}", variable: 'K8S_TOKEN')]) {
                        sh '''
                        kubectl config set-credentials jenkins-user --token=${K8S_TOKEN}
                        kubectl config set-context --current --user=jenkins-user

                        # Apply your Kubernetes manifests (e.g., deployment, service)
                        kubectl apply -f k8s/deployment.yaml
                        kubectl apply -f k8s/service.yaml
                        '''
                    }
                }
            }
        }
    }
}
