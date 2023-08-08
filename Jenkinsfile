pipeline {
    agent any

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
                sh "docker build -t $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$DOCKER_IMAGE_TAG ."
                sh "echo $DOCKER_PASSWORD | docker login --username $DOCKER_USERNAME --password-stdin"
                sh "docker push $DOCKER_REGISTRY/$DOCKER_IMAGE_NAME:$DOCKER_IMAGE_TAG"
                sh "docker logout"
            }
        }

        stage('Integrate Jenkins with EKS Cluster and Deploy App') {
            environment {
                EKS_CLUSTER_NAME = 'eks-NgNdrWhM'
                AWS_REGION = 'us-east-1'
                NAMESPACE = 'prod'
            }
            steps {
                withAWS(credentials: 'aws-credentials', region: 'us-east-1') {
                    script {
                        sh "aws eks update-kubeconfig --name $EKS_CLUSTER_NAME --region $AWS_REGION"
                        sh "kubectl apply -f manifest/namespace.yaml"
                        sh "kubectl apply -f manifest/deploy.yaml -f manifest/service.yaml -n $NAMESPACE"
                    }
                }
            }
        }
    }
}
