pipeline {
    agent { label 'my-jenkins-agent' } // 👈 Use the label of your cloud agent here

    environment {
        DOCKER_IMAGE = 'sagar4094/my-node-app'
        COMPOSE_FILE = 'docker-compose.yaml'
    }

    stages {
        stage('Clone Repo') {
            steps {
                echo '📥 Cloning repo...'
                git 'https://github.com/Sagar-Admane/test-repo.git'
            }
        }

        stage('Build Image with Compose') {
            steps {
                echo '🐳 Building Docker image...'
                sh 'docker-compose -f $COMPOSE_FILE build'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo '📤 Pushing to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                        echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                        docker tag my-node-app:latest $DOCKER_IMAGE:latest
                        docker push $DOCKER_IMAGE:latest
                    """
                }
            }
        }

        stage('Stop Old Container') {
            steps {
                echo '🛑 Stopping old container...'
                sh 'docker-compose -f $COMPOSE_FILE down || true'
            }
        }

        stage('Start New Container') {
            steps {
                echo '🚀 Starting updated container...'
                sh 'docker-compose -f $COMPOSE_FILE up -d'
            }
        }

        stage('Clean Up') {
            steps {
                echo '🧹 Cleaning unused images...'
                sh 'docker image prune -f'
            }
        }
    }
}
