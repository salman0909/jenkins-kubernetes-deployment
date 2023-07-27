pipeline {
    agent any

    environment {
        DOCKER_HUB_USERNAME = 'salman1091'
        DOCKER_IMAGE_NAME = 'nginx-example'
        DOCKER_IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Make sure to replace 'your-repo-url' with the URL of your Git repository
                    checkout([$class: 'GitSCM', userRemoteConfigs: [[url: 'https://github.com/salman0909/jenkins-kubernetes-deployment.git']]])
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_HUB_USERNAME}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} ."
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                withDockerRegistry(credentialsId: 'dockerhub-credentials', url: '') {
                    sh "docker push ${DOCKER_HUB_USERNAME}/${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                
                sh "kubectl apply -f jenkins-kubernetes-deployment/nginx-deployment.yaml"
            }
        }
    }
}

   
