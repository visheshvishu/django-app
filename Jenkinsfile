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
        
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t django-app:latest .'
                }
            }
        }
        
         stage('push to dockerhub') {
            steps {
                script{
                    // withDockerRegistry(credentialsId: 'dockerhub') {
                    //     sh 'docker push visheshvishu/django-app:latest'
                    withDockerRegistry(credentialsId: 'private-registry', url: 'https://18.234.162.5:5000')  {
                      sh 'docker tag django-app:latest 18.234.162.5:5000/django-app:latest'
                      sh 'docker push 18.234.162.5:5000/django-app:latest'
                    }
                }
            }        
        }
        
        stage('Run with Docker Compose') {
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
        
        stage('Success') {
            steps {
                echo "Deployment done!"
            }        
        }
    }
}
