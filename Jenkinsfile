pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-immg'
        IMAGE_TAG = 'latest'
    }

    stages {

        stage('Build and Push Docker Image') {
            environment {
                DOCKER_REGISTRY = 'etamarah'
                DOCKER_PASSWORD = credentials('dockerhub-cred')
                DOCKER_USERNAME = 'etamarah'
            }
            steps {
                sh 'docker build -t $DOCKER_REGISTRY/$IMAGE_NAME:$IMAGE_TAG .'
                sh 'echo $DOCKER_PASSWORD | docker login --username $DOCKER_USERNAME --password-stdin'
                sh 'docker push $DOCKER_REGISTRY/$IMAGE_NAME:$IMAGE_TAG'
                sh 'docker logout'
            }
        }

        stage('Deploy App to EKS') {
            environment {
                EKS_CLUSTER_NAME = 'eks-PItWA4sQ'
                AWS_REGION = 'us-east-2'
                NAMESPACE  = 'pre-prod'
            }
            steps {
                withAWS(credentials: 'aws-credentials', region: 'us-east-2') {
                    script {
                        sh('aws eks update-kubeconfig --name $EKS_CLUSTER_NAME --region $AWS_REGION')
                        sh 'sed -i "s/__IMAGE_NAME__/$IMAGE_NAME/" manifest/deploy.yaml'
                        sh 'sed -i "s/__TAG__/$IMAGE_TAG/" manifest/deploy.yaml'
                        sh 'sed -i "s/name_space/$NAMESPACE/" manifest/namespace.yaml'
                        sh 'kubectl apply -f manifest/namespace.yaml'
                        sh 'kubectl apply -f manifest/deploy.yaml -f manifest/service.yaml --namespace $NAMESPACE'
                    }
                }
            }
        }
    }
}
