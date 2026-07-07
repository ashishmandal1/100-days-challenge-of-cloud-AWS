# 100-days-challenge-of-cloud-AWS

My journey through the KodeKloud 100 Days Challenge of Cloud AWS.

Welcome to my Cloud and DevOps learning journey!

## Goal

To consistently learn and practice cloud technologies, AWS services, and DevOps tools through hands-on labs and projects.

## Technologies Covered

* Linux
* Shell Scripting
* Git & GitHub
* Docker
* Kubernetes
* AWS
* Terraform
* Jenkins
* Ansible
* Monitoring & Logging
* CI/CD

## Progress

* [x] Day 1 - AWS EC2 RSA Key Pair Creation (`devops-kp`)
* [x] Day 2 - AWS Security Group Creation (`devops-sg`)
* [x] Day 3 - AWS Subnet Creation (`datacenter-subnet`)
* [ ] Day 4
* [ ] Day 5
* [ ] ...
* [ ] Day 100

---

# Day 1: AWS EC2 RSA Key Pair Creation

## Task

Created an EC2 RSA key pair named `devops-kp`.

## Description

Generated a key pair to securely connect to EC2 instances using SSH authentication instead of passwords.

## Configuration

* Region: `us-east-1`
* Key Pair Name: `devops-kp`
* Key Type: `RSA`
* File Format: `.pem`

## What I Learned

* Key pairs provide secure authentication for EC2 instances.
* AWS stores the public key while users download the private key.
* Private keys must be kept secure and never committed to GitHub.
* RSA keys are commonly used for Linux instance access through SSH.

---

# Day 2: AWS Security Group Creation

## Task

Created a security group named `devops-sg` in the default VPC.

## Description

Configured a security group for Nautilus application servers to allow secure remote administration and web access.

## Configuration

* Region: `us-east-1`
* VPC: `Default VPC`
* Security Group Name: `devops-sg`

### Inbound Rules

| Type | Protocol | Port | Source    |
| ---- | -------- | ---- | --------- |
| SSH  | TCP      | 22   | 0.0.0.0/0 |
| HTTP | TCP      | 80   | 0.0.0.0/0 |

### Outbound Rules

| Type        | Protocol | Port | Destination |
| ----------- | -------- | ---- | ----------- |
| All Traffic | All      | All  | 0.0.0.0/0   |

## What I Learned

* Security Groups act as virtual firewalls in AWS.
* Inbound rules control incoming traffic.
* Outbound rules control outgoing traffic.
* HTTP uses port 80.
* SSH uses port 22.
* Default outbound rules generally allow all traffic.
* Security Groups are stateful, meaning return traffic is automatically allowed.

---

# Day 3: AWS Subnet Creation

## Task

Created a subnet named `datacenter-subnet` under the default VPC.

## Description

Provisioned a custom subnet as part of the Nautilus infrastructure migration to AWS. The subnet was created within the default VPC in the `us-east-1` region.

## Configuration

* Region: `us-east-1`
* VPC ID: `vpc-00c91d2bd56de5979`
* VPC CIDR Block: `172.31.0.0/16`
* Subnet Name: `datacenter-subnet`
* Subnet CIDR Block: `172.31.96.0/20`
* Availability Zone: `us-east-1a`

## Challenges Faced

* Encountered CIDR overlap errors because the default VPC already contained multiple `/20` subnets.
* Initially created the subnet with an extra leading space in the Name tag (`" datacenter-subnet"`), causing the lab validator to fail.
* Corrected the tag using the AWS CLI and verified the resource successfully.

## Commands Used

```bash
# Find the default VPC
aws ec2 describe-vpcs \
  --filters "Name=isDefault,Values=true" \
  --region us-east-1

# List existing subnets
aws ec2 describe-subnets \
  --region us-east-1

# Verify the subnet by name
aws ec2 describe-subnets \
  --filters "Name=tag:Name,Values=datacenter-subnet" \
  --region us-east-1
```

## What I Learned

* A VPC can contain multiple subnets across different Availability Zones.
* Subnet CIDR ranges must never overlap.
* The default VPC in AWS typically uses the `172.31.0.0/16` network.
* Resource tags must match exactly; even an extra space can break automation and validation.
* AWS CLI commands are useful for verifying resources and troubleshooting issues.
* Subnets are foundational building blocks for designing secure and scalable cloud networks.

---

## Next Goals

* Launch and connect to EC2 instances.
* Configure IAM users and roles.
* Learn VPC networking concepts in greater depth.
* Explore Infrastructure as Code with Terraform.
* Build complete DevOps workflows using AWS services.
