pipeline {
    agent any

    environment {
        IMAGE_NAME = "dockerhub_username/jenkins-nginx"
        DOCKERHUB_CREDENTIALS = "dockerhub-creds"
    }

    stages {

        stage('Clone repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Ljupce003/KIII-lab4-repo.git'
            }
        }

        stage('Build image') {
            steps {
                script {
                    sh "docker build -t $IMAGE_NAME:$BUILD_NUMBER ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKERHUB_USER',
                    passwordVariable: 'DOCKERHUB_PASS'
                )]) {
                    sh "echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin"
                }
            }
        }

        stage('Push image') {
            steps {
                script {
                    sh "docker push $IMAGE_NAME:$BUILD_NUMBER"
                }
            }
        }
    }
}