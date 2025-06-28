pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('docker-cred') // Jenkins credentials ID for DockerHub
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Shirisha95-palla/Python-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t shirisha2018/python-app:${BUILD_NUMBER} .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                sh '''
                    echo "$DOCKERHUB_CREDENTIALS_PSW" | docker login -u "$DOCKERHUB_CREDENTIALS_USR" --password-stdin
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push shirisha2018/python-app:${BUILD_NUMBER}'
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }
        success {
            slackSend (
                channel: '#all-shirisha',
                color: 'good',
                message: "✅ Docker image pushed successfully: *${env.JOB_NAME}* #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
            )
        }
        failure {
            slackSend (
                channel: '#all-shirisha',
                color: 'danger',
                message: "❌ Docker image build/push failed: *${env.JOB_NAME}* #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
            )
        }
    }
}
