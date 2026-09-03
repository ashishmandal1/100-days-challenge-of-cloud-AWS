# Day 37 - AWS EC2 S3 IAM Role Integration

* Created private S3 bucket `datacenter-s3-843070613332` in `us-east-1`.
* Enabled S3 Block Public Access.
* Created `datacenter-s3-policy` allowing `s3:PutObject`, `s3:GetObject`, and `s3:ListBucket`.
* Created IAM role `datacenter-role` with EC2 trust policy.
* Attached the S3 policy to the IAM role.
* Created an EC2 instance profile and attached the role to `datacenter-ec2`.
* Generated RSA SSH keys on `aws-client` and configured root SSH access.
* Verified EC2 assumed the `datacenter-role`.
* Successfully uploaded, listed, and downloaded `test.txt` from S3.
* Confirmed EC2-to-S3 access through IAM role permissions.
