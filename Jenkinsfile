pipeline {
    agent any
    environment {
        KUBECONFIG = credentials('your-kubeconfig-id')  // Reference your kubeconfig in Jenkins credentials
    }
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t akki2309/web-jenkins-docker-k8s .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh 'docker push akki2309/web-jenkins-docker-k8s:latest'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'DOCKER_HUB_CREDENTIALS', variable: 'KUBECONFIG')]) {
                    sh 'kubectl apply -f k8s-deployment.yaml'
                }
            }
        }
    }
}
