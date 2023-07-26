pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/salman0909/https://github.com/salman0909/jenkins-kubernetes-deployment.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = 'salman1091/nginx-example'
                    def dockerTag = 'latest'

                    docker.withRegistry('https://registry.hub.docker.com', 'docker-credentials-id') {
                        def customImage = docker.build("${dockerImage}:${dockerTag}", '.')
                        customImage.push()
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
