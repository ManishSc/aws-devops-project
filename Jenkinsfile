pipeline {
    agent any

    stages {
        stage('Pull Latest Source Code') {
            steps {
                echo 'Pulling latest code from GitHub...'
                git branch: 'main', url: 'https://github.com/ManishSc/aws-devops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t mywebsite .'
            }
        }

        stage('Stop Existing Container') {
            steps {
                echo 'Stopping existing container if running...'
                sh 'docker stop mywebsite-container || true'
            }
        }

        stage('Remove Existing Container') {
            steps {
                echo 'Removing existing container if present...'
                sh 'docker rm mywebsite-container || true'
            }
        }

        stage('Run New Container') {
            steps {
                echo 'Running new container...'
                sh 'docker run -d -p 80:80 --name mywebsite-container mywebsite'
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
