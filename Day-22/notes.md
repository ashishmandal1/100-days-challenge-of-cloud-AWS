# AWS 100 Days Challenge
## Day 22 - Secure EC2 Access using SSH Keys

### Objective
Provision a new EC2 instance and enable passwordless SSH login from the KodeKloud landing host (`aws-client`) by creating and configuring a custom SSH key.

---

## AWS Resources

- EC2 Instance
- Amazon Linux 2023
- SSH
- EC2 Instance Connect

---

## Steps Performed

### 1. Launch EC2 Instance

- Open AWS Console
- Go to EC2
- Launch Instance

Configuration:

- Name: devops-ec2
- Instance Type: t2.micro
- Region: us-east-1
- Security Group: Allow SSH (22)

Launch the instance.

---

### 2. Create SSH Key on aws-client

Switch to root.

```bash
sudo -i