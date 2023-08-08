pipeline {
    agent {
        label 'docker-node'
    }
    
    stages {
        stage('Build and Push Docker Image') {
            environment {
                DOCKER_REGISTRY = 'etamarah'
                DOCKER_IMAGE_NAME = 'flask-imgg'
                DOCKER_IMAGE_TAG = 'latest'
                DOCKER_PASSWORD = credentials('dockerhub-cred')
                DOCKER_USERNAME = 'etamarah'
            }
            steps {
                sh "podman build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$DOCKER_IMAGE_TAG ."
                sh "echo $DOCKER_PASSWORD | docker login --username $DOCKER_USERNAME --password-stdin"
                sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$DOCKER_IMAGE_TAG"
                sh "docker logout"
            }
        }
    }
}
