pipeline {
    agent any

    environment {
        IMAGE_REPO = 'akshitr801/cicd-app'
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKER_IMAGE = "${IMAGE_REPO}:${IMAGE_TAG}"
        APP_SERVER = '3.134.80.144'
        APP_USER = 'ubuntu'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Deploy to App Server') {
            steps {
                sshagent(['app-server-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no $APP_USER@$APP_SERVER "
                        docker pull $DOCKER_IMAGE &&
                        docker stop app || true &&
                        docker rm app || true &&
                        docker run -d -p 3000:3000 --name app $DOCKER_IMAGE
                    "
                    '''
                }
            }
        }
    }
}
