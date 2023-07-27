pipeline {
    agent any

    environment {
        dockerhubCredentials = 'dockerhub-credentials'
        dockerImageTag = "salman1091/nginx-example:${BUILD_TAG.toLowerCase()}"
    }

    stages {
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
                    docker.withRegistry('', dockerhubCredentials) {
                        sh "docker push $dockerImageTag"
                    }
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                sh 'curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"'
                sh "kubectl apply -f jenkins-kubernetes-deployment/nginx-deployment.yaml"
            }
        }
    }
}


   
