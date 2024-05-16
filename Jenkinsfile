pipeline {
    agent any

    triggers {
        githubPush()
    }

    environment {
        // Configure the SonarQube environment variable
        SONARQUBE_URL = 'http://your-sonarqube-url'
    }

    stages {
        stage('Git Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[url: 'https://github.com/YakirBar/Zomato-Clone.git']]])
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') { // 'SonarQube' is the name of the SonarQube server configuration in Jenkins
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=your-project-key \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=${SONARQUBE_URL}
                    """
                }
            }
        }
        // Add more stages as needed
    }

    post {
        always {
            // Optionally, wait for SonarQube analysis and quality gate status
            script {
                def qg = waitForQualityGate()
                if (qg.status != 'OK') {
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                }
            }
        }
    }
}
