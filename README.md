
Amazon EKS Infrastructure & Networking 
This repository serves as a Standard Operating Procedure for provisioning a secure, scalable Amazon EKS (v1.35) cluster. 

VPC Architecture & Networking
A production-grade EKS environment requires a high-availability network topology:

Multi-AZ Strategy: Two public subnets for Load Balancers and two private subnets for Worker Nodes across at least two Availability Zones.

DNS Resolution: Enabled for node-to-control-plane registration.

Cost Optimization: Nodes are deployed on Amazon Linux 2023 (AL2023) to take advantage of improved performance and lower overhead.

Identity & Access Management
This setup utilizes EKS Access Entries:

Cluster Role: Grants the EKS control plane permissions to manage regional AWS resources.

EKS Pod Identity: Replaces the complex OIDC/IRSA setup, allowing pods to assume IAM roles directly for AWS service interaction.

Worker Node Policies:

AmazonEKSWorkerNodePolicy

AmazonEKS_CNI_Policy

AmazonEC2ContainerRegistryReadOnly

Management & Deployment
1. Cluster Provisioning (v1.35)
Using eksctl to launch a cluster with the latest security patches:

Bash
eksctl create cluster \
  --name eks-prod-cluster \
  --version 1.35 \
  --region us-east-1 \
  --nodegroup-name standard-workers \
  --node-type t3.medium \
  --nodes 2 \
  --managed
2.Storage Management
Use the EKS Add-on for easier lifecycle management:

Bash
# Install EBS CSI Driver as an EKS Add-on
aws eks create-addon \
  --cluster-name eks-prod-cluster \
  --addon-name aws-ebs-csi-driver
3. Client Configuration
Ensure your local environment is synchronized:

Bash
# Update local KubeConfig
aws eks update-kubeconfig --name eks-prod-cluster --region us-east-1

# Verify Node Health
kubectl get nodes -o wide
Repository Status

Format: Infrastructure Playbook (Documentation Only).

Author
Jimmy96 T.

DevOps & Cloud Specialist
