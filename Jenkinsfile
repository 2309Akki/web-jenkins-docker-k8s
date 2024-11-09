pipeline {
    agent any
    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t akki2309/web-jenkins-docker-k8s .'
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_HUB_USER', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                    script {
                        sh "docker login -u $DOCKER_HUB_USER -p $DOCKER_HUB_PASSWORD"
                        sh "docker push akki2309/web-jenkins-docker-k8s:latest"
                    }
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Ensure the file exists
                    sh 'ls -al'
                    sh 'ls -al k8s-deployment.yaml'  // relative path, based on current workspace



                    // Apply Kubernetes deployment
                    sh 'kubectl apply -f k8s-deployment.yaml'

                }
            }
        }
    }
}
