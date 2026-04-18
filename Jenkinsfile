pipeline {
    agent any

    stages {
        stage ('1. Checkout code') {
            steps {
                checkout scm
            }
        }
        stage ('2. Docker image build') {
            steps {
                sh 'docker build -t sriramsrb/restaurant:latest .'
            }
        }
        stage ('3. Docker push image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub_user', variable: 'DOCKER_PWD')]) {
                    sh 'echo "$DOCKER_PWD" | docker login -u sriramsrb --password-stdin'
                    sh 'docker push sriramsrb/restaurant:latest'
                }
            }
        }
        stage ('4. deploy to kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yml'
                sh 'kubectl rollout restart deployment restaurant-app-deployment'
            }
        }
    }
}
