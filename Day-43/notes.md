# Day 43 — Amazon EKS Cluster

## Objective

Create an Amazon EKS cluster named `datacenter-eks` using the latest stable Kubernetes version, the default VPC, three availability zones, a custom configuration, and a private-only Kubernetes API endpoint.

## Configuration

* AWS Region: `us-east-1`
* Cluster Name: `datacenter-eks`
* Kubernetes Version: `1.36`
* Configuration Mode: Custom
* IAM Cluster Role: `eksClusterRole`
* Default VPC: `vpc-00fabebff09991808`
* Availability Zones:

  * `us-east-1a`
  * `us-east-1b`
  * `us-east-1c`
* EKS Auto Mode: Disabled
* Public Endpoint Access: Disabled
* Private Endpoint Access: Enabled

## Subnets

* `subnet-08a902faa71ed41aa` — `us-east-1a`
* `subnet-04c84663270e5e13f` — `us-east-1b`
* `subnet-0b45a46f9c08a64b7` — `us-east-1c`

## IAM Role

Created the `eksClusterRole` IAM role with an EKS service trust policy and attached:

* `AmazonEKSClusterPolicy`

## EKS Cluster Creation

The cluster was created with:

```bash
aws eks create-cluster \
  --name datacenter-eks \
  --kubernetes-version 1.36 \
  --role-arn "$CLUSTER_ROLE_ARN" \
  --resources-vpc-config \
    subnetIds="$SUBNET_A","$SUBNET_B","$SUBNET_C",\
endpointPublicAccess=false,\
endpointPrivateAccess=true \
  --region us-east-1
```

## Verification

Final verification confirmed:

* Cluster status: `ACTIVE`
* Kubernetes version: `1.36`
* Correct IAM role attached
* Correct default VPC
* Correct three subnets across AZs `a`, `b`, and `c`
* Private endpoint: `True`
* Public endpoint: `False`
* `computeConfig`: `null`, confirming EKS Auto Mode is not enabled

## Result

The `datacenter-eks` cluster was successfully created and verified as ready for workloads with a private Kubernetes API endpoint and high availability across three availability zones.
