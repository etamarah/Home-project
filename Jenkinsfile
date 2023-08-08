pipeline {
    agent any
    
    stages {
        stage('Build and Push Docker Image') {
            environment {
                DOCKER_REGISTRY = 'etamarah'
                DOCKER_IMAGE_NAME = 'flask-imgg'
                DOCKER_IMAGE_TAG = 'latest'
            }
            steps {
                script {
                    def dockerHubCredentials = credentials('dockerhub-cred')
                    
                    sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$DOCKER_IMAGE_TAG ."
                    sh "echo ${dockerHubCredentials.password} | docker login --username ${dockerHubCredentials.username} --password-stdin"
                    sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$DOCKER_IMAGE_TAG"
                    sh "docker logout"
                }
            }
        }
    }
}
