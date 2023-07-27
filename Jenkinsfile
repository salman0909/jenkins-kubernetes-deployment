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
        
        stage('Deploy new image') {
          steps {
            container('kubectl') {
              sh 'find'
              sh 'kubectl version'
              sh '#ls / -a'
              sh 'kubectl apply -f jenkins-kubernetes-deployment/nginx-deployment.yaml'
          }
        }
      }
    }
}


   
