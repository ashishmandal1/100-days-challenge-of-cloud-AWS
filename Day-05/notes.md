# Day 05 - Create an Amazon EBS Volume

## Lab Objective

Create an Amazon Elastic Block Store (EBS) volume with the following configuration:

- **Volume Name:** xfusion-volume
- **Volume Type:** gp3
- **Volume Size:** 2 GiB
- **Region:** us-east-1 (N. Virginia)

---

## What is Amazon EBS?

Amazon Elastic Block Store (EBS) is a block-level storage service that provides persistent storage for Amazon EC2 instances. Unlike instance storage, EBS volumes retain data even after an EC2 instance is stopped.

### Key Features

- Persistent block storage
- High availability within an Availability Zone
- Snapshots for backup and recovery
- Encryption support
- Resizable storage volumes
- Multiple volume types for different workloads

---

## Volume Type Used

### gp3 (General Purpose SSD)

- SSD-based storage
- Suitable for most workloads
- Better price-performance than gp2
- Independent configuration of IOPS and throughput
- Ideal for development, testing, and production applications

---

## Steps Performed

1. Logged in to the AWS Management Console.
2. Switched to the **us-east-1** region.
3. Opened the **EC2** service.
4. Navigated to **Elastic Block Store → Volumes**.
5. Clicked **Create Volume**.
6. Configured the volume:
   - Volume Type: **gp3**
   - Size: **2 GiB**
   - Availability Zone: **us-east-1a**
7. Added the following tag:
   - Key: `Name`
   - Value: `xfusion-volume`
8. Clicked **Create Volume**.
9. Verified that the volume was successfully created.

---

## Lab Configuration

| Property | Value |
|----------|-------|
| Name | xfusion-volume |
| Volume Type | gp3 |
| Size | 2 GiB |
| Region | us-east-1 |
| State | Available |

---

## Commands Used

No AWS CLI commands were required because this lab was completed using the AWS Management Console.

---

## Outcome

Successfully created a **2 GiB gp3 Amazon EBS volume** named **xfusion-volume** in the **us-east-1** region.

---

## Key Takeaways

- Learned how to create an Amazon EBS volume.
- Understood the purpose of block storage in AWS.
- Learned about the advantages of the gp3 volume type.
- Practiced configuring storage resources using the AWS Management Console.

## Status

✅ Day 5 Completed