pipeline {
    agent { label 'my-jenkins-agent' } // Your custom cloud agent

    environment {
        DOCKER_HOST = 'tcp://172.22.0.2:2376' // 👈 Connect to Docker daemon via TCP
        DOCKER_IMAGE = 'sagar4094/my-node-app' // Docker Hub image name
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo '📥 Cloning the repository...'
                git 'https://github.com/Sagar-Admane/test-repo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo '🐳 Building the Docker image...'
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                echo '📤 Logging in & pushing to Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                        echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                        docker push $DOCKER_IMAGE
                    """
                }
            }
        }

        stage('Stop Old Container') {
            steps {
                echo '🛑 Stopping existing container (if any)...'
                sh 'docker stop my-node-app || true'
                sh 'docker rm my-node-app || true'
            }
        }

        stage('Run New Container') {
            steps {
                echo '🚀 Running new container...'
                sh 'docker run -d --name my-node-app -p 3000:3000 $DOCKER_IMAGE'
            }
        }

        stage('Clean Up Old Images') {
            steps {
                echo '🧹 Cleaning up unused images...'
                sh 'docker image prune -f'
            }
        }
    }
}
