pipeline{
    agent any

    environment{
        DOCKER_IMAGE = 'my-node-app'
    }

    stages{
        stage('Clone repo'){
            steps{
                git 'https://github.com/Sagar-Admane/test-repo.git'
            }
        }

        stage('Build docker image with docker compose'){
            steps{
                script{
                    sh 'docker-compose build'
                }
            }
        }

        stage('Stop old container'){
            steps{
                script{
                    sh 'docker-compose down'
                }
            }
        }

        stage('Start new container'){
            steps{
                script{
                    sh 'docker-compose up -d'
                }
            }
        }

        stage('Clean docker images'){
            steps{
                script{
                    sh 'docker image prune -f'
                }
            }
        }
    }
}