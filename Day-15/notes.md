# AWS 100 Days Challenge - Day 15

## Topic
Create an Amazon EBS Snapshot

## Objective
Create a snapshot of an existing EBS volume to back up its data.

## Resources Used
- Amazon EC2
- Amazon EBS Volume
- Amazon EBS Snapshot

## Steps Performed
1. Logged into the AWS Management Console.
2. Switched to the **us-east-1** region.
3. Opened **EC2 → Elastic Block Store → Volumes**.
4. Selected the existing volume **datacenter-vol**.
5. Clicked **Actions → Create snapshot**.
6. Entered the following details:
   - **Name:** `datacenter-vol-ss`
   - **Description:** `datacenter Snapshot`
7. Created the snapshot.
8. Verified that the snapshot status changed to **Completed**.

## Outcome
Successfully created a snapshot named **datacenter-vol-ss** for the EBS volume **datacenter-vol**.