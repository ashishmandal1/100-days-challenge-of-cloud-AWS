# Day 06 - Launch an Amazon EC2 Instance

## Objective
Launch an Amazon EC2 instance with the required configuration.

## Services Used
- Amazon EC2
- Amazon Machine Image (AMI)
- Amazon EBS
- Security Groups
- EC2 Key Pair

## Configuration

- Instance Name: nautilus-ec2
- AMI: Amazon Linux
- Instance Type: t2.micro
- Key Pair: nautilus-kp (RSA)
- Security Group: default
- Region: us-east-1

## Steps Performed

1. Logged into the AWS Management Console.
2. Opened the EC2 Dashboard.
3. Clicked Launch Instance.
4. Entered the instance name as **nautilus-ec2**.
5. Selected the Amazon Linux AMI.
6. Chose the **t2.micro** instance type.
7. Created a new RSA key pair named **nautilus-kp**.
8. Selected the existing **default** security group.
9. Launched the EC2 instance.
10. Verified that the instance entered the **Running** state.

## Outcome

Successfully launched an Amazon EC2 instance with the required configuration.

## Key Learnings

- Amazon EC2 provides scalable virtual servers in the cloud.
- A key pair is used for secure SSH access to Linux instances.
- Security groups act as virtual firewalls for EC2 instances.
- Every EC2 instance launches with an attached root EBS volume.

## Status

✅ Day 5 Completed