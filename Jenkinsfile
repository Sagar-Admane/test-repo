pipeline {
    agent { label 'my-jenkins-agent' }

    environment {
        DOCKER_IMAGE = 'sagar4094/my-node-app'
    }

    stages {
        stage('Clone Repo') {
            steps {
                echo '📥 Cloning repo...'
                git 'https://github.com/Sagar-Admane/test-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building Docker image from Dockerfile...'
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo '📤 Pushing to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                        echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                        docker tag $DOCKER_IMAGE:latest $DOCKER_IMAGE:latest
                        docker push $DOCKER_IMAGE:latest
                    """
                }
            }
        }

        stage('Stop Old Container') {
            steps {
                echo '🛑 Stopping old container...'
                sh 'docker ps -q --filter "name=my-node-app" | xargs -r docker stop'
                sh 'docker ps -aq --filter "name=my-node-app" | xargs -r docker rm'
            }
        }

        stage('Start New Container') {
            steps {
                echo '🚀 Starting updated container...'
                sh 'docker run -d --name my-node-app -p 80:80 $DOCKER_IMAGE'
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
