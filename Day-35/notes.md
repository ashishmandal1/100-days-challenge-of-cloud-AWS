# Day 35 - Private RDS MySQL with EC2 PHP Application

## Objective

Create a private MySQL RDS database and connect an existing EC2 instance to it using a PHP application.

## Resources Created

* **RDS Instance:** `datacenter-rds`
* **Engine:** MySQL 8.4.5
* **Instance Class:** `db.t3.micro`
* **Storage:** 5 GiB `gp2`
* **Database:** `datacenter_db`
* **Master Username:** `datacenter_admin`
* **Public Access:** Disabled
* **RDS Security Group:** `datacenter-rds-sg`
* **RDS Port:** `3306`
* **EC2 Instance:** `datacenter-ec2`
* **EC2 Security Group:** `sg-02985e25281b10797`
* **HTTP Port:** `80`

## Steps Performed

### 1. Verified AWS Region

```bash
aws configure get region
```

Region used:

```text
us-east-1
```

### 2. Identified the Existing EC2

The existing `datacenter-ec2` instance was identified along with its VPC, subnet, private IP, public IP, and security group.

```text
Instance ID: i-0170c95a9f7ba4fbc
Private IP: 172.31.38.143
Public IP: 3.90.52.10
Security Group: sg-02985e25281b10797
```

### 3. Updated EC2 Security Group

Opened HTTP port 80:

```bash
aws ec2 authorize-security-group-ingress \
  --group-id sg-02985e25281b10797 \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0
```

### 4. Created RDS Security Group

Created:

```text
datacenter-rds-sg
```

Then allowed MySQL traffic from the EC2 security group:

```bash
aws ec2 authorize-security-group-ingress \
  --group-id sg-0249b4f0af24f6abf \
  --protocol tcp \
  --port 3306 \
  --source-group sg-02985e25281b10797
```

This allows:

```text
datacenter-ec2 → TCP 3306 → datacenter-rds
```

### 5. Created Private RDS Instance

Created `datacenter-rds` using MySQL 8.4.5:

```bash
aws rds create-db-instance \
  --db-instance-identifier datacenter-rds \
  --engine mysql \
  --engine-version 8.4.5 \
  --db-instance-class db.t3.micro \
  --allocated-storage 5 \
  --storage-type gp2 \
  --master-username datacenter_admin \
  --master-user-password 'DatacenterAdmin2026!' \
  --db-name datacenter_db \
  --vpc-security-group-ids sg-0249b4f0af24f6abf \
  --no-publicly-accessible \
  --backup-retention-period 0 \
  --region us-east-1
```

The final RDS state was:

```text
Status: available
Engine: MySQL 8.4.5
Class: db.t3.micro
Storage: 5 GiB
Storage Type: gp2
Database: datacenter_db
Port: 3306
Publicly Accessible: False
```

### 6. Created SSH Key

On the KodeKloud AWS client:

```bash
ssh-keygen -t rsa -b 4096 -f /root/.ssh/id_rsa -N ""
```

The key pair was created at:

```text
/root/.ssh/id_rsa
/root/.ssh/id_rsa.pub
```

The public key was added to the root user's authorized keys on `datacenter-ec2`.

### 7. Verified Passwordless SSH

Connected from the AWS client to the EC2:

```bash
ssh root@3.90.52.10
```

Verified:

```bash
whoami
```

Output:

```text
root
```

### 8. Copied PHP Application

The existing application was located at:

```text
/root/index.php
```

Copied it to the EC2:

```bash
scp /root/index.php root@3.90.52.10:/var/www/html/index.php
```

### 9. Configured PHP for RDS

Updated `/var/www/html/index.php` with:

```php
$dbname = 'datacenter_db';
$dbuser = 'datacenter_admin';
$dbpass = 'DatacenterAdmin2026!';
$dbhost = 'datacenter-rds.ccpukl8e8cys.us-east-1.rds.amazonaws.com';
```

### 10. Verified Database Connectivity

Installed/verified the MySQL client and connected from the EC2:

```bash
mysql -h datacenter-rds.ccpukl8e8cys.us-east-1.rds.amazonaws.com \
  -u datacenter_admin \
  -p
```

Then:

```sql
USE datacenter_db;
SHOW TABLES;
```

The database connection succeeded. The database contained no tables, which was expected.

### 11. Verified Apache

```bash
systemctl status apache2 --no-pager
```

Apache was confirmed:

```text
Active: active (running)
```

## Final Architecture

```text
Internet
    |
    | HTTP :80
    v
datacenter-ec2
    |
    | MySQL :3306
    v
Private datacenter-rds
    |
    v
datacenter_db
```

## Key Concepts Learned

* Amazon RDS
* Private RDS instances
* MySQL on RDS
* RDS security groups
* EC2 security groups
* Security group-to-security group access
* MySQL port 3306
* Private database connectivity
* SSH public/private keys
* Passwordless SSH
* `scp` file transfer
* Apache web server
* PHP `mysqli` database connectivity
* EC2-to-RDS architecture
* AWS CLI RDS management

## Verification

The final application successfully connected from the EC2 instance to the private RDS database and displayed:

```text
Connected successfully
```

**Status: Day 35 Completed ✅**
