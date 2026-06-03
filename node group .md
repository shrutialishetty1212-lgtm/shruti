# Terraform AWS EKS Cluster Deployment

This project provisions an Amazon EKS (Elastic Kubernetes Service) cluster and managed node group using Terraform.

## Architecture

- AWS EKS Cluster
- Managed Node Group
- Default VPC
- Default Subnets
- IAM Roles and Policies

## Prerequisites

Before running this project, ensure you have:

- AWS Account
- AWS CLI installed and configured
- Terraform installed (v1.5+ recommended)
- IAM user with permissions for:
  - EKS
  - EC2
  - IAM
  - VPC

## Configure AWS Credentials

```bash
aws configure
```

Provide:

```text
AWS Access Key ID
AWS Secret Access Key
Default Region: ap-south-1
Output Format: json
```

## Project Structure

```text
.
├── main.tf
├── terraform.tfstate
├── terraform.tfstate.backup
└── README.md
```

## Resources Created

### EKS Cluster

- Cluster Name: `my-eks-cluster`
- Kubernetes Version: `1.33`
- Uses Default VPC and Subnets

### Node Group

- Node Group Name: `my-node-group`
- Instance Type: `t3.medium`
- Desired Nodes: `2`
- Minimum Nodes: `1`
- Maximum Nodes: `3`

### IAM Roles

#### Cluster Role

Attached Policy:

- AmazonEKSClusterPolicy

#### Node Group Role

Attached Policies:

- AmazonEKSWorkerNodePolicy
- AmazonEKS_CNI_Policy
- AmazonEC2ContainerRegistryReadOnly

## Initialize Terraform

```bash
terraform init
```

## Validate Configuration

```bash
terraform validate
```

## Preview Changes

```bash
terraform plan
```

## Deploy Infrastructure

```bash
terraform apply -auto-approve
```

Terraform will create:

- EKS Cluster
- Node Group
- IAM Roles
- Policy Attachments

## Verify Cluster

List clusters:

```bash
aws eks list-clusters --region ap-south-1
```

Update kubeconfig:

```bash
aws eks update-kubeconfig \
--region ap-south-1 \
--name my-eks-cluster
```

Verify connection:

```bash
kubectl get nodes
```

Expected Output:

```text
NAME                              STATUS   ROLES    AGE   VERSION
ip-xxx-xxx-xxx-xxx.ec2.internal   Ready    <none>   xxm   v1.33.x
```

## Outputs

Terraform outputs:

```bash
terraform output
```

Example:

```text
cluster_name = "my-eks-cluster"
node_group_name = "my-node-group"
```

## Destroy Infrastructure

To avoid AWS charges:

```bash
terraform destroy -auto-approve
```

## Notes

- This project uses the AWS Default VPC.
- Suitable for learning and testing purposes.
- For production environments:
  - Use custom VPCs
  - Private subnets
  - Managed security groups
  - IAM Roles for Service Accounts (IRSA)
  - Cluster Autoscaler
  - Monitoring and Logging

## Author

Created using Terraform and AWS EKS.
