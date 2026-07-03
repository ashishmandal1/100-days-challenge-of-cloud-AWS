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

* [x] Day 1 - AWS EC2 RSA Key Pair Creation
* [x] Day 2 - AWS Security Group Creation (devops-sg)
* [ ] Day 3
* [ ] Day 4
* [ ] Day 5
* [ ] ...
* [ ] Day 100

---

## Day 2: AWS Security Group Creation

### Task
Created a security group named `devops-sg` in the default VPC.

### Description
Security group for Nautilus App Servers

### Configuration
- Region: `us-east-1`
- VPC: `Default VPC`
- HTTP (TCP 80) → `0.0.0.0/0`
- SSH (TCP 22) → `0.0.0.0/0`

### What I Learned
- Security Groups act as virtual firewalls in AWS.
- Inbound rules control incoming traffic.
- HTTP uses port 80.
- SSH uses port 22.
- Default outbound rules should generally be left unchanged.