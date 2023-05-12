# eks-set-up
1) Create Dedicated VPC For EKS Cluster. Create Dedicated VPC For EKS Cluster. When you create a cluster, the VPC that you specify must meet the following requirements and considerations:
    
1.1) The VPC must have a sufficient number of IP addresses available for the cluster, any nodes, and other Kubernetes resources that you want to create

1.2)The VPC must have DNS hostname and DNS resolution support. Otherwise, nodes can't register to your cluster.

1.3) This VPC has two public and two private subnets. A public subnet's associated route table has a route to an internet gateway. However, the route table of a private subnet doesn’t have a route to an internet gateway. One public and one private subnet are deployed to the same Availability Zone. The other public and private subnets are deployed to a second Availability Zone in the same AWS Region. We recommend this option for most deployments.With this option, you can deploy your nodes to private subnets. This option allows Kubernetes to deploy load balancers to the public subnets that can load balance traffic to pods that run on nodes in the private subnets. Public IPv4 addresses are automatically assigned to nodes that are deployed to public subnets, but public IPv4 addresses aren't assigned to nodes deployed to private subnets.

     
2) Create IAM Role For EKS Cluster.
      EKS – Cluster   
      eks-demo-iam-role

3) Create EKS Cluster.
   GUI - Console
  commands:
      aws eks create-cluster  
      eksctl create-cluster 

      aws s3 ls 
      aws eks list-clusters 
  IaC = tERRAFORM

4) Create IAM Role For EKS Worker Nodes with the policies below.
    
        AmazonEKSWorkerNodePolicy
        AmazonEKS_CNI_Policy
        AmazonEC2ContainerRegistryReadOnly
        AmazonEBSCSIDriverPolicy 

5) Create Worker Nodes.


6) Create An Instance (If Not Exists) Install AWS CLI , IAM Authenticator And kubectl. Configure AWS CLI using Root or IAM User Access Key & Secret Key. Or Attach IAM With Required       Policies.


####Setup K8s Client Machine #####

### Install Kubectl In Linux====

1) Install Download the latest kubectl release with the command:

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"



2) Make the kubectl binary executable. 

     chmod +x ./kubectl
   
3) Move the binary in to your PATH.

      sudo mv ./kubectl /usr/local/bin/kubectl
4) Test to ensure the version you installed is up-to-date:

kubectl version --client   


### Install aws CLI In Linux====

1) Download AWS CLI ZIP
    
  curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"

2) Download & Install Unzip
    sudo yum install unzip -y

3) Extract Zip 
    unzip awscliv2.zip
  
4) Install
  sudo ./aws/install -i /usr/local/aws-cli -b /usr/local/bin
  
5) Verify
  aws --version 
  
  
######## Configure AWS CLI using ACCESS Key & Secret Key ########

aws configure
  AK*********BSER
  
  cmN*****************U
  
  us-west-1
  
  json

.##### Get KubeConfig file #####

aws eks update-kubeconfig --name <ClusterName> --region <RegionName> 

aws eks update-kubeconfig --name eks-demo --region us-west-1 

##### Verify Kubectl #####
kubectl get nodes
kubectl get pods


# Install git if not already installed before exeucting below command as we are applying k8s manifests of EBSCSI driver direclyt from git hub.

sudo yum install git -y 


Deploy AWSEBSSCSI Driver Plugin(For Dynamic Volumes with EBS StoragClass)
=========================================================================
kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=release-1.12"

