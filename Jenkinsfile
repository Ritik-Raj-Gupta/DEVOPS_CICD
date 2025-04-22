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
                    sh 'docker compose -f $COMPOSE_FILE build'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Deploying application...'
                    sh 'docker compose -f $COMPOSE_FILE up -d'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up containers...'
            sh 'docker compose -f $COMPOSE_FILE down || true'
        }

        success {
            echo '✅ Pipeline succeeded!'
        }

        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
