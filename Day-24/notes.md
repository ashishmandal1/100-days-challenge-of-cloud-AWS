## Day 24 - Application Load Balancer (ALB)

### Task
Create an Application Load Balancer in front of an existing EC2 instance running Nginx.

### Services Used
- Amazon EC2
- Application Load Balancer (ALB)
- Target Groups
- Security Groups

### What I Did
- Created an Internet-facing Application Load Balancer (`datacenter-alb`)
- Created a Target Group (`datacenter-tg`)
- Registered the EC2 instance (`datacenter-ec2`)
- Created a Security Group (`datacenter-sg`) allowing HTTP traffic
- Updated the EC2 Security Group to allow HTTP traffic from the ALB
- Configured the ALB Listener to forward traffic to the Target Group
- Verified the Target Group health status as **Healthy**
- Successfully accessed the Nginx page using the ALB DNS

### Key Learning
- Layer 7 Load Balancing
- Target Groups
- Health Checks
- Listener Configuration
- Security Group Communication
- High Availability with ALB