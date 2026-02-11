
Amazon EKS Infrastructure & Networking Setup

This repository provides a step-by-step blueprint for configuring a production-ready Amazon EKS (Elastic Kubernetes Service) environment. It covers VPC networking requirements, IAM security policies, and the installation of essential Kubernetes drivers.

1. VPC Networking Requirements
A dedicated VPC must be created to satisfy the following EKS requirements:

IP Availability: Sufficient IP addresses for nodes, pods, and resources.

DNS Support: DNS hostnames and resolution must be enabled for node registration.

Subnet Strategy: A 4-subnet architecture (2 Public, 2 Private) across two Availability Zones.

Nodes reside in private subnets for security.

Load Balancers reside in public subnets to route external traffic.

2. Identity & Access Management (IAM)
Proper roles are required for the cluster to interact with AWS services:

EKS Cluster Role
Create an IAM role (e.g., eks-demo-iam-role) to allow the EKS control plane to manage resources on your behalf.

Worker Node Policies
Worker nodes must be attached to a role containing these specific policies:

AmazonEKSWorkerNodePolicy: Core node functionality.

AmazonEKS_CNI_Policy: Networking interface support.

AmazonEC2ContainerRegistryReadOnly: Ability to pull images from ECR.

AmazonEBSCSIDriverPolicy: Permission to manage EBS storage volumes.

3. Client Machine Configuration
To manage the cluster, your local or EC2-based management machine requires the following tools:

Install Kubectl
Bash

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
Install AWS CLI & Configure
Bash

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws configure
4. Cluster Connectivity & Storage
Once the cluster is created via the Console or eksctl, connect your client:

Update KubeConfig
Bash

aws eks update-kubeconfig --name eks-demo --region us-west-1
Deploy Amazon EBS CSI Driver
Necessary for dynamic volume provisioning using EBS storage classes:

Bash

kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-cs-driver/deploy/kubernetes/overlays/stable/?ref=release-1.12"

Author
Jimmy96 T.

DevOps & Cloud Specialist

