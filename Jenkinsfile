pipeline {
    agent any

    environment {
        // Set the path for your docker-compose.yml file
        COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the code from GitHub
                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build Docker images using docker-compose
                    sh 'docker-compose -f docker-compose.yml build'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy the app to your server or cloud
                    // Here you might push to Docker Hub or deploy to a remote server
                    sh 'docker-compose -f $COMPOSE_FILE up -d'
                }
            }
        }
    }

    post {
        always {
            // Clean up: Make sure to stop containers and remove any unused volumes
            sh 'docker-compose -f $COMPOSE_FILE down'
        }

        success {
            echo 'Pipeline succeeded!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}
}
