pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "raveendra137/captain"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$TAG .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-credentials',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE:$TAG'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "Updating deployment with new image..."
                
                kubectl set image deployment/cap-app flask=$DOCKER_IMAGE:$TAG
                
                echo "Applying YAML (in case changes)..."
                kubectl apply -f capappdeployment.yaml
                kubectl apply -f capservice.yaml

                echo "Waiting for rollout..."
                kubectl rollout status deployment/cap-app
                '''
            }
        }
    }
}



