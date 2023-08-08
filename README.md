project pre-quisites
==============================
1. A github account 
An EC2 instance for with Docker installed(Docker Server)
An EC2 instance for with Jenkins installed(Jenkins Server)
An EC2 instance for with Ansible installed(Ansible Server)
An EC2 instance for with Terraform Installed(Terraform Server)
A prouction grade Kubernetes cluster ( i have included a terraform script of provisioning one)
a Dockerhub Account 

This README.md file describes the files in this repository and how to use them.

EKS/eks.tf

This file contains the Terraform configuration for creating an EKS cluster. It uses the `terraform-aws-modules/eks/aws` module.

To create the cluster, run the following commands:

"terraform init" - to initialize the backend and terraform registry also provide the requires plugins and download modules.
"terraform validate" to check and validate the syntax for terraform configurations.
"terraform plan" to do a dry run preview the infrastructures that terraform is about to create and also provide an execution plan to create them
"terraform apply" to provision or modify the resource outline in the terraform plan.
```

Deployment/deploy.yaml

This file contains the Kubernetes deployment configuration for the Flask app. It defines a deployment with 3 replicas, a service, and a liveness probe.

To deploy the app, run the following command:


kubectl apply -f deploy.yaml


Jenkinsfile

This file contains the Jenkinsfile for the CI/CD pipeline. It uses the `docker-node` and `ansible` agents.

The pipeline first checks out the source code from GitHub, then builds a Docker image, pushes it to DockerHub, and deploys the app to EKS.

To run the pipeline, click the "Build Now" button on the Jenkins dashboard.
To automates the build process so that developers can see changes as soon as a new version of the code is pushed to the repository i configured a github webhook push event to the project repository which automatically triggers a build anytime a new version of the code is push into the repository. 

How to use this code

To use this code, you will need to have the following installed and integrated together:

* Terraform
* Kubernetes
* Jenkins
* Docker

Once you have all of the required software installed, you can follow these steps:

1. Clone the repository to your local machine.
2. Create a new Terraform configuration file for your environment.
3. Run `terraform init` to initialize the Terraform configuration.
4. Run `terraform apply` to create the EKS cluster.
5. Create a new Kubernetes deployment file for your app.
6. Run `kubectl apply -f deploy.yaml` to deploy the app to EKS.
7. Create a new Jenkinsfile for your CI/CD pipeline.
8. Run `jenkinsfile build` to run the pipeline.

Once the pipeline has completed, your app will be deployed to EKS.
