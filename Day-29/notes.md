# Day 29 – AWS VPC Peering

## 📌 Overview

This lab demonstrates how to establish private communication between two Amazon VPCs using **VPC Peering**.

The environment consists of:

* A default/public VPC containing a publicly accessible EC2 instance.
* A private VPC containing a private EC2 instance.
* A VPC Peering Connection between the two VPCs.
* Route tables configured on both sides.
* Security groups configured to allow ICMP traffic.
* SSH access configured for the public EC2 instance.

The final connectivity test confirmed successful communication from the public EC2 instance to the private EC2 instance.

---

## 🎯 Objective

The objective was to:

1. Identify the existing public and private VPCs.
2. Identify the existing EC2 instances.
3. Create a VPC Peering Connection.
4. Accept and name the peering connection.
5. Configure routes in both VPCs.
6. Configure security groups.
7. Establish SSH access to the public EC2 instance.
8. Add the AWS client's public SSH key to `authorized_keys`.
9. SSH into the public EC2.
10. Ping the private EC2 through the VPC Peering Connection.

---

## 🏗️ Architecture

```text
                         AWS Client
                      65.108.255.62
                            |
                            | SSH
                            v
              ┌─────────────────────────┐
              │       Default VPC       │
              │       172.31.0.0/16     │
              │                         │
              │  Public EC2             │
              │  172.31.34.124          │
              │  54.144.106.138         │
              └────────────┬────────────┘
                           |
                           | VPC Peering
                           | pcx-04d5f39a4b8813b5b
                           |
              ┌────────────▼────────────┐
              │       Private VPC       │
              │        10.1.0.0/16      │
              │                         │
              │  Private Subnet         │
              │    10.1.1.0/24          │
              │                         │
              │  Private EC2            │
              │    10.1.1.246           │
              └─────────────────────────┘
```

---

## ☁️ AWS Resources

### Public VPC

```text
VPC ID: vpc-0ca6aa75c0148f26f
CIDR: 172.31.0.0/16
```

### Public EC2

```text
Name: nautilus-public-ec2
Instance ID: i-0ae416a0e7350fc02
Private IP: 172.31.34.124
Public IP: 54.144.106.138
Security Group: sg-09aff78d018fa386b
```

### Private VPC

```text
Name: nautilus-private-vpc
VPC ID: vpc-020950b52c72e02cc
CIDR: 10.1.0.0/16
```

### Private Subnet

```text
Subnet ID: subnet-0c524eca6c853a2e2
CIDR: 10.1.1.0/24
```

### Private EC2

```text
Name: nautilus-private-ec2
Instance ID: i-02ee7ac97ec8ca81a
Private IP: 10.1.1.246
Security Group: sg-0e8b310d448fcba6a
```

### VPC Peering

```text
Name: nautilus-vpc-peering
Peering ID: pcx-04d5f39a4b8813b5b
Status: active
```

---

## 🔗 VPC Peering

Created the peering connection:

```bash
aws ec2 create-vpc-peering-connection \
  --region us-east-1 \
  --vpc-id vpc-0ca6aa75c0148f26f \
  --peer-vpc-id vpc-020950b52c72e02cc
```

Accepted the peering connection:

```bash
aws ec2 accept-vpc-peering-connection \
  --region us-east-1 \
  --vpc-peering-connection-id pcx-04d5f39a4b8813b5b
```

Verified that the status became:

```text
active
```

Named the peering connection:

```bash
aws ec2 create-tags \
  --region us-east-1 \
  --resources pcx-04d5f39a4b8813b5b \
  --tags Key=Name,Value=nautilus-vpc-peering
```

---

## 🛣️ Route Configuration

### Public VPC → Private VPC

```bash
aws ec2 create-route \
  --region us-east-1 \
  --route-table-id rtb-0c64b034530789622 \
  --destination-cidr-block 10.1.0.0/16 \
  --vpc-peering-connection-id pcx-04d5f39a4b8813b5b
```

### Private VPC → Public VPC

```bash
aws ec2 create-route \
  --region us-east-1 \
  --route-table-id rtb-017eeb281aad7b464 \
  --destination-cidr-block 172.31.0.0/16 \
  --vpc-peering-connection-id pcx-04d5f39a4b8813b5b
```

Both directions were required because network communication needs a route for the request and the return traffic.

---

## 🔐 Security Group Configuration

Allowed ICMP traffic from the public VPC to the private EC2:

```bash
aws ec2 authorize-security-group-ingress \
  --region us-east-1 \
  --group-id sg-0e8b310d448fcba6a \
  --protocol icmp \
  --port -1 \
  --cidr 172.31.0.0/16
```

Allowed SSH from the AWS client:

```bash
aws ec2 authorize-security-group-ingress \
  --region us-east-1 \
  --group-id sg-09aff78d018fa386b \
  --protocol tcp \
  --port 22 \
  --cidr 65.108.255.62/32
```

Using `/32` limits SSH access to the specific client IP.

---

## 🔑 SSH Configuration

The public EC2 did not have an existing EC2 key pair.

EC2 Instance Connect was used to establish initial access:

```bash
aws ec2-instance-connect send-ssh-public-key \
  --region us-east-1 \
  --instance-id i-0ae416a0e7350fc02 \
  --availability-zone us-east-1a \
  --instance-os-user ec2-user \
  --ssh-public-key file:///root/.ssh/id_rsa.pub
```

The AWS client's public key was then permanently added to:

```text
~/.ssh/authorized_keys
```

with the appropriate permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

SSH was verified using:

```bash
ssh -i /root/.ssh/id_rsa \
  -o IdentitiesOnly=yes \
  -o StrictHostKeyChecking=no \
  ec2-user@54.144.106.138
```

---

## 🧪 Connectivity Test

From the public EC2:

```bash
ping -c 4 10.1.1.246
```

Result:

```text
4 packets transmitted, 4 received, 0% packet loss
```

### ✅ Verification

```text
Public EC2
172.31.34.124
       |
       | ICMP
       ↓
VPC Peering
       |
       ↓
Private EC2
10.1.1.246
```

The successful ping confirmed that VPC Peering, routing, and security group configuration were working correctly.

---

## 📚 Key Concepts Learned

* VPC Peering
* VPC CIDR blocks
* Route tables
* Security groups
* ICMP
* SSH public-key authentication
* EC2 Instance Connect
* Private IP communication
* AWS CLI networking commands
* Bidirectional routing

---

## ⚠️ Troubleshooting Lessons

### SSH Permission Denied

```text
Permission denied (publickey)
```

Possible causes include:

* Incorrect private key.
* Incorrect username.
* SSH port 22 not allowed.
* Public key missing from `authorized_keys`.
* Incorrect `.ssh` permissions.

For persistent SSH access:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

### Ping Failure

When ping fails, verify:

1. Peering status is `active`.
2. Public route table contains `10.1.0.0/16`.
3. Private route table contains `172.31.0.0/16`.
4. Private EC2 security group allows ICMP.
5. Both EC2 instances are running.
6. The destination private IP is correct.

---

## 💡 Important Lesson

A successful VPC Peering connection alone does **not** guarantee communication.

The complete path requires:

```text
VPC Peering
     ↓
Route Table
     ↓
Security Group
     ↓
Application/Protocol
     ↓
Connectivity Test
```

For SSH-based lab verification, temporary EC2 Instance Connect access should not be confused with permanent SSH key configuration when the task specifically requires `authorized_keys`.

---

## 🏆 Outcome

Successfully configured **VPC Peering** between the default VPC and the private VPC.

The public EC2 instance successfully connected to the private EC2 instance using its private IP address:

```text
10.1.1.246
```

Final connectivity result:

```text
4 packets transmitted
4 packets received
0% packet loss
```

**Day 29 AWS VPC Peering lab completed successfully. ✅**
