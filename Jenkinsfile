pipeline {
    agent any

    environment {
        // Path to docker compose file
        COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Clone repository
                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building Docker images...'
                    sh 'docker-compose build'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up containers...'
            sh 'docker-compose down || true'
        }

        success {
            echo '✅ Pipeline succeeded!'
        }

        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
