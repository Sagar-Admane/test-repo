pipeline {
    agent any

    tools {
        nodejs 'node16' 
    }

    trigger{
        pollSCM '* * * * *'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/Sagar-Admane/test-repo.git'
            }
        }

        stage('Run Script') {
            steps {
                echo 'Running index.js...'
                sh 'node index.js'
            }
        }
    }
}
