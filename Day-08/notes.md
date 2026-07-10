# Day 08 – Enable EC2 Stop Protection

## Objective

Enable Stop Protection for an existing Amazon EC2 instance to prevent it from being accidentally stopped.

## Services Used

* Amazon EC2

## Task Performed

* Logged in to the AWS Management Console.
* Switched to the **us-east-1 (N. Virginia)** region.
* Opened the **EC2** service.
* Selected the EC2 instance named **datacenter-ec2**.
* Navigated to **Actions → Instance settings → Change stop protection**.
* Enabled **Stop Protection** for the instance.
* Verified that Stop Protection was successfully enabled.

## What I Learned

* Stop Protection helps prevent accidental stopping of an EC2 instance.
* It is useful for protecting production or critical workloads.
* Stop Protection can be enabled or disabled through the AWS Management Console or AWS CLI.
* This feature only prevents the **Stop** action; it does not prevent termination unless Termination Protection is also enabled.

## Status

✅ Day 5 Completed