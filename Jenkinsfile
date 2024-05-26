pipeline {
    agent {
        label 'inbound-agent'
    }

    stages {
        stage('Check Docker Version') {
            steps {
                script {
                    // Check if Docker is installed and print the Docker version
                    try {
                        sh 'docker --version'
                    } catch (Exception e) {
                        // Handle the case where Docker is not installed
                        echo 'Docker is not installed on this agent.'
                    }
                }
            }
        }
    }
}
