pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/salman0909/your-repo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = 'your-dockerhub-username/your-image-name'
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
                sh "kubectl apply -f path/to/your/deployment.yaml"
            }
        }
    }
}
