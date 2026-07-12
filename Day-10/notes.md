# Day 10 - Associate an Elastic IP with an EC2 Instance

## Objective

Attach an existing Elastic IP to an existing EC2 instance in the AWS us-east-1 region.

## Resources

- EC2 Instance: datacenter-ec2
- Elastic IP: datacenter-ec2-eip
- Region: us-east-1

## Steps Performed

1. Logged into the AWS Management Console.
2. Switched to the us-east-1 region.
3. Opened the EC2 Dashboard.
4. Navigated to **Elastic IPs**.
5. Selected the Elastic IP named **datacenter-ec2-eip**.
6. Chose **Actions → Associate Elastic IP address**.
7. Selected the EC2 instance **datacenter-ec2**.
8. Confirmed the association.

## Result

- Successfully associated the Elastic IP **datacenter-ec2-eip** with the EC2 instance **datacenter-ec2**.

## Key Learnings

- Elastic IPs provide a static public IPv4 address for AWS resources.
- An Elastic IP can be associated with an EC2 instance or a network interface.
- Associating an Elastic IP allows the instance to retain the same public IP even after stop/start operations.
- Elastic IPs should be released when no longer needed to avoid unnecessary charges.

## Status

✅ Day 5 Completed