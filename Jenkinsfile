pipeline {
    agent any

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
                    def dockerImage = 'salman1091/nginx-example'
                    def dockerTag = 'latest'

                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
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
