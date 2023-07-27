pipeline {
    agent any
    kubernetes {
            // podTemplate with containers and volumes will be defined here
            podTemplate {
                containers {
                    container(name: 'docker', image: 'docker:latest', ttyEnabled: true)
                }
                volumes {
                    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock', name: 'docker-socket')
                }
            }
    }
    environment {
        dockerImageTag = "salman1091/nginx-example:${BUILD_TAG.toLowerCase()}"
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
                script {
                    sh "docker --version"
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
