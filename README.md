# Nginx-deployment-EKS-cluster
Deploy nginx container on EKS
Deploying Nginx on an Amazon EKS (Elastic Kubernetes Service) cluster involves a series of steps to ensure that Nginx is properly set up and running in your Kubernetes environment. Here's a comprehensive guide to help you through the process:

Prerequisites:
1. Amazon EKS Cluster: You need an existing EKS cluster. If you don't have one, you can create it using the AWS Management Console, AWS CLI, or eksctl.
2. kubectl: The Kubernetes command-line tool must be installed and configured to interact with your EKS cluster.
3. aws-cli: Ensure AWS CLI is installed and configured with the necessary permissions.

Steps to Deploy Nginx on EKS
1. Set Up Your Environment
Ensure you have the necessary tools installed and configured:
# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl

# Install aws-cli
pip install awscli --upgrade --user

# Install eksctl
curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/0.108.0/eksctl_Linux_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
Configure kubectl to use your EKS cluster:
aws eks --region <region> update-kubeconfig --name <cluster-name>

2.Deploy Nginx Using a Deployment:
Create a YAML file for the Nginx deployment. You can name it nginx-deployment.yaml:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: nginx
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

  Apply :
  kubectl apply -f nginx-deployment.yaml
3.create service for load balancer:
Kubectl Create Service Loadbalancer Nginx --Tcp=80:80

4. Set Up IAM Roles and Policies

EKS requires IAM roles and policies for both the control plane and worker nodes. eksctl handles this for you, but if you need to manually set up IAM roles, you can follow these steps:

Create an IAM Role for EKS Cluster:
Go to the IAM console.
Create a new role with the AmazonEKSClusterPolicy and AmazonEKSServicePolicy attached.

Create an IAM Role for EKS Node Group:
Create a new role with the AmazonEKSWorkerNodePolicy, AmazonEC2ContainerRegistryReadOnly, and AmazonEKS_CNI_Policy attached.

>>>>>>>Connect kubectl to an EKS cluster by creating a kubeconfig file:<<<<<<<<<<<<<<<<<<<<<<<<<
To create your kubeconfig file with the AWS CLI
Create or update a kubeconfig file for your cluster. Replace region-code with the AWS Region that your cluster is in and replace my-cluster with the name of your cluster.
aws eks update-kubeconfig --region region-code --name my-cluster
Test your configuration.
kubectl get svc
kubectl get pods
kubectl delete pod <pod-name>
kubectl get services
kubectl delete service <service-name>
























