pipeline {
    agent any

    stages {
        stage {
            steps ('1. Checkout code') {
                checkout scm
            }
        }
        stage {
            steps ('2. Docker image build') {
                sh 'docker build -t sriramsrb/restaurant:latest .'
            }
        }
        stage {
            steps ('3. Docker push image') {
                withCredentials([string(credentialId: 'dockerhub_user', variable: 'DOCKER_PWD')]) {
                    sh 'echo "$DOCKER_PWD" | docker login -u sriramsrb --password-stdin'
                    sh 'docker push sriramsrb/restaurant:latest'
                }
            }
        }
        stage {
            steps ('4. deploy to kubernetes') {
                sh 'kubectl apply -f deployment.yml'
                sh 'kubectl rollout restart deployment restaurant-app-deployment'
            }
        }
    }
}