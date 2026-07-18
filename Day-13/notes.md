# Day 13 - Create an Amazon Machine Image (AMI)

## Objective

Create an Amazon Machine Image (AMI) from an existing EC2 instance.

## What is an AMI?

An Amazon Machine Image (AMI) is a template used to launch EC2 instances. It contains:

- Operating System
- Application software
- Configurations
- Block device mappings

## Why use AMIs?

- Backup an EC2 instance
- Launch identical instances
- Disaster recovery
- Auto Scaling
- Faster deployments

## Steps Performed

1. Opened the EC2 Console.
2. Selected the instance `datacenter-ec2`.
3. Clicked:
   - Actions
   - Image and templates
   - Create image
4. Entered the image name:
   - `datacenter-ec2-ami`
5. Created the image.
6. Waited until the AMI state became **Available**.

## AWS CLI Commands

Get instance ID:

```bash
aws ec2 describe-instances \
--filters "Name=tag:Name,Values=datacenter-ec2" \
--query "Reservations[].Instances[].InstanceId" \
--output text
```

Create AMI:

```bash
aws ec2 create-image \
--instance-id <INSTANCE_ID> \
--name "datacenter-ec2-ami"
```

Check AMI status:

```bash
aws ec2 describe-images \
--owners self \
--filters "Name=name,Values=datacenter-ec2-ami" \
--query "Images[].State"
```

## Key Learnings

- AMIs are reusable templates for EC2 instances.
- AMIs preserve the configuration of an EC2 instance.
- An AMI must reach the **Available** state before it can be used.
- AMIs simplify scaling, migration, and disaster recovery.

## Outcome

Successfully created an AMI named **datacenter-ec2-ami** from the EC2 instance **datacenter-ec2**.

## Status

✅ Day 13 Completed