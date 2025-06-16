pipeline {
    agent any
    environment {
        COMPOSE_FILE = 'docker-compose.yml'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    echo 'Building Docker image...'

                    // Build the streamlit image and tag it
                    sh "docker build -t streamlit_app:${IMAGE_TAG} ."
                }
            }
        }

        stage('Update Compose File') {
            steps {
                script {
                    echo 'Updating docker-compose.yml with new image tag...'
                    
                    // Replace the image tag for streamlit service in docker-compose.yml
                    sh "sed -i 's|build: .|image: streamlit_app:${IMAGE_TAG}|' docker-compose.yml"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    echo 'Stopping any existing containers...'
                    sh 'docker-compose down -v --remove-orphans || true'

                    echo 'Deploying application...'
                    sh 'docker-compose up -d'
                }
            }
        }
        stage('SmokeTest') {
    steps {
        sh 'curl -f http://localhost:8501 && echo "Success" || echo "Failure"'
    }
}
    }
}
