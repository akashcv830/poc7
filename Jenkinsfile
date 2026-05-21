pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "472004/poc7-app"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Deploy using Ansible') {
    steps {
        sh '''
        ansible-playbook ansible/deploy.yml \
        -i ansible/hosts \
        --private-key /root/key.pem
        '''
    }
}

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'poc7',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    '''
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }
    }
}
