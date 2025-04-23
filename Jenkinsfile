pipeline {
    agent any

    environment {
        COMPOSE_FILE = 'docker-compose.yml'
    }

    stages {
        stage('Clone Code from GitHub') {
            steps {
                git branch: 'main', url: 'https://github.com/visheshvishu/django-app.git'
            }
        }
        
        stage('Check Docker Version') {
            steps {
                sh 'docker --version || echo "Docker not installed"'
                sh 'docker compose --version || echo "Docker Compose not installed"'
            }
        }
        
        stage('Build and Run with Docker Compose') {
            steps {
                sh '''
                    if [ -f "$COMPOSE_FILE" ]; then
                        docker compose down
                        docker compose up --build -d
                    else
                        echo "$COMPOSE_FILE not found!"
                        exit 1
                    fi
                '''
            }
        }
        
        stage('push to dockerhub') {
            steps {
                script{
                    withDockerRegistry(credentialsId: 'dockerhub') {
                        sh 'docker push visheshvishu/django-app:latest'
                    }
                }
            }        
        }
        
        stage('Success') {
            steps {
                echo "Deployment done!"
            }        
        }
    }
}
