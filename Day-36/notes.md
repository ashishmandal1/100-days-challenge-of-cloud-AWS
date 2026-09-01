# Day 36 - AWS Application Load Balancer

## Objective
Configured an EC2 instance with Nginx and integrated it with an Application Load Balancer.

## Resources Created
- EC2: `nautilus-ec2`
- Security Group: `nautilus-sg`
- Target Group: `nautilus-tg`
- Application Load Balancer: `nautilus-alb`
- Listener: HTTP port 80

## Configuration
- Used Ubuntu 24.04 LTS AMI.
- Launched EC2 using `t2.micro`.
- Installed and started Nginx using user data.
- Allowed port 80 from the ALB default security group.
- Configured ALB with two Availability Zones.
- Registered EC2 on port 80 with the target group.
- Configured ALB listener to forward HTTP traffic to the target group.
- Verified target health as `healthy`.

## Verification
Tested the ALB DNS using curl and received:

HTTP/1.1 200 OK

Server: nginx/1.24.0 (Ubuntu)

## Result
Successfully accessed the Nginx web server through the Application Load Balancer.