pipeline {
    agent any

    stages {
        stage ('1. Checkout code SCM') {
            steps {
                checkout scm
            }
        }
        stage ('2. Docker build image') {
            steps {
                sh 'docker build -t sriramsrb/library-app:latest .'
            }
        }
        stage ('3. Docker push image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub_user', variable: 'DOCKER_PWD')]) {
                    sh 'echo "$DOCKER_PWD" | docker login -u sriramsrb --password-stdin'
                    sh 'docker build -t sriramsrb/library-app:latest'
                }
            }
        }
        stage ('4. Deploy to kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yml'
                sh 'kubectl rollout restart deplpyment ibrary-app-deployment'
            }
        }
    }
}
