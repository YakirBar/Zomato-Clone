pipeline {
    agent {
        label 'inbound-agent'
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('Docker Hub Access Token')
    }
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t yakirbartech/node-app:latest .'
            }
        }
        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push') {
            steps {
                sh 'docker push yakirbartech/node-app:latest'
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
