# Day 27 - Create a Public VPC with EC2 Instance

## Objective
Create a public VPC, configure a public subnet with automatic public IP assignment, attach an Internet Gateway, configure routing, create a Security Group allowing SSH access, and launch an EC2 instance.

## Services Used
- Amazon VPC
- Internet Gateway
- Route Table
- Public Subnet
- Security Group
- Amazon EC2

## Resources Created
- VPC: `devops-pub-vpc`
- Internet Gateway: `devops-pub-igw`
- Route Table: `devops-pub-rt`
- Public Subnet: `devops-pub-subnet`
- Security Group: `devops-pub-sg`
- EC2 Instance: `devops-pub-ec2`

## Steps Performed
1. Created a custom VPC.
2. Created and attached an Internet Gateway.
3. Created a public subnet.
4. Enabled Auto Assign Public IPv4.
5. Created a custom Route Table.
6. Added a default route (`0.0.0.0/0`) to the Internet Gateway.
7. Associated the subnet with the Route Table.
8. Created a Security Group allowing SSH (TCP 22).
9. Launched a `t2.micro` EC2 instance inside the public subnet.
10. Verified the instance received a public IPv4 address.

## Outcome
Successfully deployed a public AWS infrastructure consisting of a custom VPC, public subnet, Internet Gateway, route table, Security Group, and an EC2 instance accessible via SSH over the internet.

## Key Learnings
- Custom VPC creation
- Public subnet configuration
- Internet Gateway attachment
- Route table associations
- Security Group configuration
- Launching EC2 in a custom network
- Public IP assignment