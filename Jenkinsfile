pipeline {
    agent any

    environment {
        APP_SERVER = "ec2-user@10.0.2.161"
        SSH_KEY = "/var/lib/jenkins/.ssh/jenkins_to_app"
    }

    stages {
        stage('Pull Latest Source Code') {
            steps {
                echo 'Pulling latest code from GitHub...'
                git branch: 'main', url: 'https://github.com/ManishSc/aws-devops-project.git'
            }
        }

        stage('Copy Files to App Server') {
            steps {
                echo 'Copying source code to App Server...'
                sh "scp -i ${SSH_KEY} -o StrictHostKeyChecking=no -r ./* ${APP_SERVER}:/website/"
            }
        }

        stage('Stop Existing Docker Container') {
            steps {
                echo 'Stopping existing container on App Server...'
                sh "ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${APP_SERVER} 'docker stop mywebsite-container || true'"
            }
        }

        stage('Remove Existing Docker Container') {
            steps {
                echo 'Removing existing container on App Server...'
                sh "ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${APP_SERVER} 'docker rm mywebsite-container || true'"
            }
        }

        stage('Build Latest Docker Image') {
            steps {
                echo 'Building Docker image on App Server...'
                sh "ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${APP_SERVER} 'cd /website && docker build -t mywebsite .'"
            }
        }

        stage('Run New Docker Container') {
            steps {
                echo 'Running new container on App Server with EFS mounted inside...'
                sh "ssh -i ${SSH_KEY} -o StrictHostKeyChecking=no ${APP_SERVER} 'docker run -d -p 80:80 -v /website:/usr/share/nginx/html --name mywebsite-container mywebsite'"
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
