pipeline {
    agent any

    stages {
        stage('Build and Push Docker Image') {
            environment {
                DOCKER_REGISTRY = 'etamarah'
                DOCKER_IMAGE_NAME = 'flask-cs'
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

        stage('Integrate Jenkins with EKS Cluster and Deploy App') {
            environment {
                EKS_CLUSTER_NAME = 'eks-NgNdrWhM'
                AWS_REGION = 'us-east-1'
                NAMESPACE = 'prod'
            }
            steps {
                script {
                    withAWS(credentials: 'aws-credentials', region: 'us-east-1') {
                        sh "aws eks update-kubeconfig --name $EKS_CLUSTER_NAME --region $AWS_REGION"
                        sh "ls && ls deployment"
                        sh "kubectl apply -f namespace.yaml"
                        sh "kubectl apply -f deploy.yaml -f service.yaml -n $NAMESPACE"
                    }
                }
            }
        }
    }
}
