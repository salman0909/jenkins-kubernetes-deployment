pipeline {
    agent any
    environment {
        dockerImageTag = "salman1091/my-web-app:${BUILD_TAG.toLowerCase()}"
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Make sure to replace 'your-repo-url' with the URL of your Git repository
                    checkout([$class: 'GitSCM', userRemoteConfigs: [[url: 'https://github.com/salman0909/my-web-app.git']]])
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $dockerImageTag ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                        sh "docker push $dockerImageTag"
                    }
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
