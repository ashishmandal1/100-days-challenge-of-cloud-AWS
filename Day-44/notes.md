# Day 44 — Auto Scaling Group and Application Load Balancer

## Objective

Set up a highly available Nginx web application using an AWS Auto Scaling Group (ASG) and Application Load Balancer (ALB). Configure automatic scaling based on average CPU utilization with a target of 50%.

## Resources Created

* VPC: `vpc-0a9dff54b3affd21a`
* Security Group: `devops-sg`
* Security Group ID: `sg-094384586cb1f717e`
* Launch Template: `devops-launch-template`
* Launch Template ID: `lt-01187d72fd893e455`
* Auto Scaling Group: `devops-asg`
* Target Group: `devops-tg`
* Application Load Balancer: `devops-alb`
* EC2 Instance: `i-08c9daef41b588f24`

## Launch Template Configuration

* AMI: Amazon Linux 2023
* AMI ID: `ami-0354c98ae10b02961`
* Instance Type: `t2.micro`
* Security Group: `devops-sg`
* HTTP Port: `80`

## User Data

The launch template uses User Data to automatically:

1. Update packages.
2. Install Nginx.
3. Start the Nginx service.
4. Enable Nginx to start automatically on boot.

## Auto Scaling Configuration

* ASG Name: `devops-asg`
* Minimum Capacity: `1`
* Desired Capacity: `1`
* Maximum Capacity: `2`
* Health Check Type: `ELB`
* Health Check Grace Period: `300 seconds`
* Scaling Policy: Target Tracking
* Metric: `ASGAverageCPUUtilization`
* Target CPU Utilization: `50%`

## Load Balancer Configuration

* ALB Name: `devops-alb`
* Scheme: Internet-facing
* Type: Application Load Balancer
* Listener: HTTP port `80`
* Target Group: `devops-tg`
* Target Protocol: HTTP
* Target Port: `80`
* Health Check Path: `/`
* Health Check Protocol: HTTP
* Healthy Threshold: `2`
* Unhealthy Threshold: `2`

## Verification

Target health was verified successfully:

```text
i-08c9daef41b588f24
Port: 80
State: healthy
```

ASG instance status:

```text
Instance: i-08c9daef41b588f24
Lifecycle: InService
Health: Healthy
```

ALB HTTP test:

```text
HTTP/1.1 200 OK
Server: nginx/1.30.4
```

The ALB returned the default Nginx page containing:

```text
Welcome to nginx!
```

## ALB DNS

`devops-alb-1601827546.us-east-1.elb.amazonaws.com`

## Key Concepts Learned

* Launch Templates
* Auto Scaling Groups
* Target Tracking Scaling
* CPU-based automatic scaling
* Application Load Balancers
* Target Groups
* ALB health checks
* Multi-AZ deployment
* Nginx installation using EC2 User Data
* ASG and ALB integration
* End-to-end application verification
