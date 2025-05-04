pipeline{
    agent any
    stages {
        stage('Init'){
            steps {
                echo 'pipline from github'
            }
        }
        stage("Clone repo"){
            steps{
                git 'https://github.com/Sagar-Admane/test-repo.git'
            }
        }

        stage("Run Scripts"){
            steps{
                echo "Running index.js"
                sh 'node index.js'
            }
        }
    }
}