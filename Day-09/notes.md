# Day 09 - Enable EC2 Termination Protection

## Objective

Enable termination protection for an existing EC2 instance named `nautilus-ec2` in the `us-east-1` region.

## Steps Performed

1. Logged in to the AWS Management Console.
2. Navigated to **EC2 > Instances**.
3. Selected the existing instance `nautilus-ec2`.
4. Opened **Actions > Instance settings > Change termination protection**.
5. Enabled **Termination Protection**.
6. Saved the changes.

## Result

Termination protection was successfully enabled for the `nautilus-ec2` instance, preventing accidental termination through the AWS Management Console or API.

## Key Learning

- Termination protection helps prevent accidental deletion of important EC2 instances.
- This setting can be enabled or disabled without stopping the instance.
- It is a recommended best practice for production or critical workloads.

## Status

✅ Day 5 Completed