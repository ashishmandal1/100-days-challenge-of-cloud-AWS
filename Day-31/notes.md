# Day 31 — AWS RDS MySQL

## 📌 Overview

On Day 31 of the AWS 100 Days Challenge, I provisioned a **private Amazon RDS MySQL database** using the AWS CLI.

The database was configured using the Free Tier requirements and placed inside a DB subnet group spanning multiple Availability Zones.

## 🎯 Objective

Create a private RDS instance named `devops-rds` with:

* MySQL 8.4.x
* `db.t3.micro`
* 20 GiB gp2 storage
* Storage autoscaling up to 22 GiB
* Public access disabled
* Region `us-east-1`

## 🏗️ Resources Created

### RDS Instance

```text
Identifier:      devops-rds
Engine:          MySQL
Version:         8.4.11
Instance Class:  db.t3.micro
Storage:         20 GiB
Storage Type:    gp2
Max Storage:     22 GiB
Public Access:   Disabled
Region:          us-east-1
Status:          available
```

### DB Subnet Group

```text
Name: devops-rds-subnet-group
VPC:  vpc-01d71db0728d62760
```

Subnets:

```text
subnet-008dc8cfb0a6d8b7f → us-east-1a
subnet-02cee6ae4d9a1c2f7 → us-east-1b
```

## 🔧 AWS CLI

### Check RDS

```bash
aws rds describe-db-instances \
  --db-instance-identifier devops-rds \
  --region us-east-1
```

### Create DB Subnet Group

```bash
aws rds create-db-subnet-group \
  --db-subnet-group-name devops-rds-subnet-group \
  --db-subnet-group-description "Private subnet group for devops-rds" \
  --subnet-ids subnet-008dc8cfb0a6d8b7f subnet-02cee6ae4d9a1c2f7 \
  --region us-east-1
```

### Create RDS Instance

```bash
aws rds create-db-instance \
  --db-instance-identifier devops-rds \
  --engine mysql \
  --engine-version 8.4.11 \
  --db-instance-class db.t3.micro \
  --allocated-storage 20 \
  --storage-type gp2 \
  --max-allocated-storage 22 \
  --master-username admin \
  --master-user-password 'DevopsRDS2026!' \
  --db-subnet-group-name devops-rds-subnet-group \
  --no-publicly-accessible \
  --backup-retention-period 0 \
  --region us-east-1
```

### Verify Configuration

```bash
aws rds describe-db-instances \
  --db-instance-identifier devops-rds \
  --region us-east-1 \
  --query 'DBInstances[0].{Status:DBInstanceStatus,Engine:Engine,Version:EngineVersion,Class:DBInstanceClass,Storage:AllocatedStorage,StorageType:StorageType,MaxStorage:MaxAllocatedStorage,Public:PubliclyAccessible}' \
  --output table
```

## ✅ Final Result

The RDS instance successfully reached:

```text
Status:       available
Engine:       mysql
Version:      8.4.11
Class:        db.t3.micro
Storage:      20 GiB
Storage Type: gp2
Max Storage:  22 GiB
Public:       False
```

## 🧠 Key Learnings

1. How to create an RDS instance using AWS CLI.
2. How to configure MySQL 8.4.x.
3. How to create and use an RDS DB subnet group.
4. How to make an RDS instance private.
5. How to configure gp2 storage.
6. How to enable RDS storage autoscaling.
7. How to set a maximum storage threshold.
8. How to monitor the RDS instance until it reaches the `available` state.

## 🏆 Status

**Day 31 — Completed ✅**
