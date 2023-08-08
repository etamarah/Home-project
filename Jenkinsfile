pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-cred')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from the Git repository
                git "https://github.com/etamarah/Home-project.git"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t etamarah/flask_app:$BUILD_NUMBER ."
            }
        }

        stage('Login to DockerHub') {
            steps {
                sh "echo \$DOCKERHUB_CREDENTIALS_PSW | docker login -u \$DOCKERHUB_CREDENTIALS_USR --password-stdin"
            }
        }

        stage('Push Image to DockerHub') {
            steps {
                sh "docker push etamarah/flask_app:$BUILD_NUMBER"
            }
        }

        stage('Deploy App to EKS') {
            agent { label "ansible" }
            steps {
                withCredentials([
                    [
                        $class: 'AmazonWebServicesCredentialsBinding',
                        credentialsId: 'aws-cred', 
                        accesskeyVariable: 'AWS_ACCESS_KEY_ID',
                        secretkeyVariable: 'AWS_SECRET_ACCESS_KEY'
                    ]
                ]) {
                    sh 'aws eks update-kubeconfig --name $EKS_CLUSTER_NAME --region $AWS_REGION'
                    sh 'sed -i "s/__IMAGE_NAME__/$IMAGE_NAME/" deploy.yaml'
                    sh 'kubectl apply -f /My_Project/Deployment/deploy.yaml'
                }
            }
        }
    }
}

