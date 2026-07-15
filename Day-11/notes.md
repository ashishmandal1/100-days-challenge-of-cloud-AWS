# Day 11 - Attach an Elastic Network Interface (ENI) to an EC2 Instance

## Objective

Attach an existing Elastic Network Interface (ENI) named `nautilus-eni` to an existing EC2 instance named `nautilus-ec2` in the `us-east-1` region.

---

## What is an Elastic Network Interface (ENI)?

An Elastic Network Interface (ENI) is a virtual network adapter that can be attached to an EC2 instance. It enables advanced networking capabilities, such as multiple private IP addresses, security groups, and the ability to move network interfaces between instances.

---

## Steps Performed

1. Logged in to the AWS Management Console.
2. Selected the **US East (N. Virginia)** (`us-east-1`) region.
3. Opened the **EC2 Dashboard**.
4. Verified that the EC2 instance `nautilus-ec2` was running and had completed initialization.
5. Navigated to **Network Interfaces**.
6. Selected the existing network interface `nautilus-eni`.
7. Clicked **Actions → Attach**.
8. Selected the EC2 instance `nautilus-ec2`.
9. Used **Device Index 1** for the attachment.
10. Confirmed the network interface status changed to **In use (Attached)**.

---

## AWS Services Used

- Amazon EC2
- Elastic Network Interface (ENI)

---

## Key Learnings

- An ENI acts as a virtual network card for an EC2 instance.
- Multiple ENIs can be attached to a single EC2 instance depending on the instance type.
- ENIs and EC2 instances must reside in the same Availability Zone.
- The primary network interface cannot be detached while the instance is running.
- ENIs retain their private IP addresses and security groups even when detached and reattached.

---

## Outcome

Successfully attached the existing Elastic Network Interface (`nautilus-eni`) to the EC2 instance (`nautilus-ec2`) and verified that the interface status changed to **Attached/In Use**.

## Status

✅ Day 5 Completed