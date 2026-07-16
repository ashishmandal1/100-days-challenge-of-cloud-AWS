# Day 12 - Attach an EBS Volume to an EC2 Instance

## 🎯 Objective
Learn how to attach an existing Amazon Elastic Block Store (EBS) volume to an EC2 instance for additional storage.

## 🛠️ Services Used
- Amazon EC2
- Amazon EBS

## 📚 What I Did
- Attached an existing EBS volume to an EC2 instance.
- Verified that the volume attachment was successful.
- Learned how AWS uses device names when attaching storage.
- Understood that attaching a volume only makes it available to the instance and does not automatically make it usable inside the operating system.

## ✅ Outcome
Successfully attached the EBS volume to the target EC2 instance.

## 📖 Key Learnings
- EBS provides persistent block storage for EC2 instances.
- A volume must be attached before it can be mounted and used.
- Each attached volume requires a unique device name.
- EBS volumes should be in the same Availability Zone as the EC2 instance.

## Status

✅ Day 5 Completed