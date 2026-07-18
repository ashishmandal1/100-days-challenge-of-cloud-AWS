# Day 14 - Terminate EC2 Instance

## Objective
Learn how to safely terminate an Amazon EC2 instance when it is no longer required.

## What I Learned
- An EC2 instance can be permanently deleted by terminating it.
- Before termination, termination protection must be disabled if it is enabled.
- After termination:
  - The instance cannot be recovered.
  - Attached instance store data is lost permanently.
  - Root EBS volume is deleted by default (unless configured otherwise).
  - Elastic IPs associated with the instance are disassociated.
- Terminating unused instances helps reduce cloud costs.

## Steps Performed
1. Opened the AWS Management Console.
2. Navigated to **EC2 Dashboard**.
3. Selected the target EC2 instance.
4. Verified the correct instance was selected.
5. Clicked **Instance State → Terminate (Delete) Instance**.
6. Confirmed the termination request.
7. Waited until the instance state changed to **Terminated**.

## Key Concepts
- EC2 Instance Lifecycle
- Instance Termination
- Cost Optimization
- Resource Cleanup
- Termination Protection

## Outcome
Successfully terminated the specified EC2 instance.