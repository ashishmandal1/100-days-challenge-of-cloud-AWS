# Day 3 - AWS Subnet Creation

## Task
Created a subnet named `datacenter-subnet` under the default VPC in `us-east-1`.

## Details
- VPC: Default VPC
- Subnet Name: datacenter-subnet
- CIDR Block: 172.31.96.0/20
- Region: us-east-1

## Commands Used

```bash
aws ec2 describe-vpcs --filters "Name=isDefault,Values=true" --region us-east-1

aws ec2 create-tags \
  --resources subnet-0c461235b64f3ce92 \
  --tags Key=Name,Value=datacenter-subnet \
  --region us-east-1
