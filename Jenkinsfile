pipeline {
    agent any
    environment {
        IMAGE_NAME = 'shirisha2018/python-app:v1'
        TAG = '1'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Shirisha95-palla/Python-app.git', branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$TAG .'
            }
        }
        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME:$TAG'
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
