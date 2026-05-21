pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/akashcv830/poc7.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t poc7-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker stop poc7 || true
                docker rm poc7 || true
                docker run -d -p 8081:80 --name poc7 poc7-app
                '''
            }
        }
    }
}

