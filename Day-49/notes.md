# Day 49 — Secure Log Aggregation with VPC Peering, EC2, IAM and S3

## Objective

Build a secure and scalable log aggregation pipeline where a log file from an EC2 instance in a private VPC is transferred to an EC2 instance in a public VPC and then uploaded to a private S3 bucket using an IAM role.

## Architecture

Private EC2 → VPC Peering → Public EC2 → S3

```text
Private VPC
10.10.0.0/16
     |
     | 10.10.0.0/16 ↔ 10.20.0.0/16
     | VPC Peering
     ↓
Public VPC
10.20.0.0/16
     |
     ↓
Private S3 Bucket
```

## AWS Resources

### Existing Private VPC

* VPC: `datacenter-priv-vpc`
* VPC CIDR: `10.10.0.0/16`
* Subnet: `datacenter-priv-subnet`
* Subnet CIDR: `10.10.1.0/24`
* Route table: `datacenter-priv-rt`
* EC2: `datacenter-priv-ec2`
* Private IP: `10.10.1.95`
* Key pair: `datacenter-key`

### Public VPC

* VPC: `datacenter-pub-vpc`
* VPC CIDR: `10.20.0.0/16`
* Subnet: `datacenter-pub-subnet`
* Subnet CIDR: `10.20.1.0/24`
* Route table: `datacenter-pub-rt`
* Internet Gateway attached
* Public route: `0.0.0.0/0 → Internet Gateway`
* Public EC2: `datacenter-pub-ec2`
* Private IP: `10.20.1.28`
* Public IP: `44.203.119.56`

### VPC Peering

* Peering connection: `datacenter-vpc-peering`
* Status: `active`
* Private VPC route:
  `10.20.0.0/16 → VPC Peering`
* Public VPC route:
  `10.10.0.0/16 → VPC Peering`

### IAM

* IAM role: `datacenter-s3-role`
* Instance profile: `datacenter-s3-instance-profile`
* Permission:
  `s3:PutObject`
* The role was attached to `datacenter-pub-ec2`.

The role was verified from the EC2 instance using:

```bash
aws sts get-caller-identity
```

The returned identity was the assumed `datacenter-s3-role`.

### S3

* Bucket: `datacenter-s3-logs-747070694`
* Region: `us-east-1`
* Bucket is private.
* Public access block:

  * `BlockPublicAcls: true`
  * `IgnorePublicAcls: true`
  * `BlockPublicPolicy: true`
  * `RestrictPublicBuckets: true`

Required S3 object:

```text
datacenter-priv-vpc/boot/boots.log
```

## Log Transfer

The source log file was:

```text
/var/log/boots.log
```

Content:

```text
Hello from datacenter KKE!
```

The private EC2 transfers the file to the public EC2 using SCP:

```bash
scp -i /home/ubuntu/datacenter-key.pem \
  -o StrictHostKeyChecking=no \
  /var/log/boots.log \
  ubuntu@10.20.1.28:/home/ubuntu/boots.log
```

The transfer was manually tested successfully.

## Private EC2 Cron Job

The private EC2 runs the SCP transfer every minute:

```cron
*/1 * * * * /usr/bin/scp -i /home/ubuntu/datacenter-key.pem -o StrictHostKeyChecking=no /var/log/boots.log ubuntu@10.20.1.28:/home/ubuntu/boots.log >> /home/ubuntu/scp-upload.log 2>&1
```

## Public EC2 Cron Job

The public EC2 uploads the received log to S3 every minute:

```cron
*/1 * * * * /usr/bin/aws s3 cp /home/ubuntu/boots.log s3://datacenter-s3-logs-747070694/datacenter-priv-vpc/boot/boots.log --region us-east-1 >> /home/ubuntu/s3-upload.log 2>&1
```

The AWS CLI uses the EC2 instance profile and does not require static AWS access keys.

## Verification

### Private → Public transfer

Verified successfully:

```text
/var/log/boots.log
        ↓ SCP
/home/ubuntu/boots.log
```

File size:

```text
27 bytes
```

### Public → S3 upload

Verified successfully:

```bash
aws s3 ls \
  s3://datacenter-s3-logs-747070694/datacenter-priv-vpc/boot/ \
  --region us-east-1
```

Result:

```text
2026-09-24 01:42:03    27    boots.log
```

### S3 Object Verification

Verified:

```text
ContentLength: 27
ServerSideEncryption: AES256
```

### Security Verification

S3 public access blocking was verified with all four settings enabled:

```text
BlockPublicAcls: true
IgnorePublicAcls: true
BlockPublicPolicy: true
RestrictPublicBuckets: true
```

## Key Concepts Learned

* Creating and configuring a VPC
* Public and private subnets
* Internet Gateway
* Route tables and routes
* VPC Peering
* Inter-VPC private networking
* EC2 networking
* Security Groups
* SSH and SCP
* IAM roles for EC2
* EC2 instance profiles
* Instance Metadata Service (IMDS)
* IAM-based S3 access
* S3 private bucket configuration
* S3 server-side encryption
* Linux cron jobs
* Automated log transfer
* End-to-end cloud automation

## Final Architecture

```text
                 AWS
                  |
       ┌──────────┴──────────┐
       │                     │
 Private VPC             Public VPC
10.10.0.0/16            10.20.0.0/16
       │                     │
       │                     │
 Private EC2             Public EC2
10.10.1.95              10.20.1.28
       │                     │
       │   VPC Peering       │
       └─────────────────────┘
                  │
                  │ SCP
                  ▼
          /home/ubuntu/boots.log
                  │
                  │ AWS CLI
                  │ IAM Role
                  ▼
          Private S3 Bucket
      datacenter-s3-logs-747070694
                  │
                  ▼
datacenter-priv-vpc/boot/boots.log
```

## Status

**Day 49 — Completed and fully verified.**
