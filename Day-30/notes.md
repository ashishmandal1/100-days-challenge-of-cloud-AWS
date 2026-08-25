# Day 30 — NAT Instance for Private EC2 Internet Access

## Objective

Configure internet access for an EC2 instance running in a private subnet using a **NAT Instance** instead of a NAT Gateway.

The private EC2 instance was configured with a cron job that uploads `xfusion-test.txt` to the public S3 bucket `xfusion-nat-32213` every minute. Successful upload confirms that internet access through the NAT Instance is working.

## AWS Resources

| Resource            | Value                   |
| ------------------- | ----------------------- |
| Region              | `us-east-1`             |
| VPC                 | `xfusion-priv-vpc`      |
| VPC ID              | `vpc-00150491408dcab62` |
| VPC CIDR            | `10.1.0.0/16`           |
| Private Subnet      | `xfusion-priv-subnet`   |
| Private Subnet CIDR | `10.1.1.0/24`           |
| Public Subnet       | `xfusion-pub-subnet`    |
| Public Subnet CIDR  | `10.1.2.0/24`           |
| Private EC2         | `xfusion-priv-ec2`      |
| Private EC2 IP      | `10.1.1.209`            |
| NAT Instance        | `xfusion-nat-instance`  |
| NAT Instance ID     | `i-0dddf73974a34288d`   |
| NAT Private IP      | `10.1.2.216`            |
| NAT Public IP       | `54.197.18.148`         |
| NAT Security Group  | `sg-07542a5de38dec1d7`  |
| Internet Gateway    | `igw-0bcb2619d77b93f16` |
| Public Route Table  | `rtb-0da721b6d3cf8c811` |
| Private Route Table | `rtb-0f1199cf07000bb7a` |
| S3 Bucket           | `xfusion-nat-32213`     |

## Architecture

```text
                         Internet
                            │
                            ▼
                  ┌──────────────────┐
                  │ Internet Gateway │
                  └────────┬─────────┘
                           │
                           ▼
                  Public Route Table
                   0.0.0.0/0 → IGW
                           │
                           ▼
              ┌─────────────────────────┐
              │      NAT Instance       │
              │   10.1.2.216            │
              │   Public IP assigned    │
              │   iptables + MASQUERADE │
              └───────────┬─────────────┘
                          │
                          ▼
                 Private Route Table
                  0.0.0.0/0 → NAT
                          │
                          ▼
              ┌─────────────────────────┐
              │     Private EC2         │
              │     10.1.1.209          │
              └───────────┬─────────────┘
                          │
                          ▼
                    Amazon S3
             xfusion-nat-32213
```

## Implementation

### 1. Created Public Subnet

Created:

```text
Name: xfusion-pub-subnet
CIDR: 10.1.2.0/24
AZ: us-east-1a
```

### 2. Created Internet Gateway

Created and attached:

```text
igw-0bcb2619d77b93f16
```

### 3. Created Public Route Table

Configured:

```text
10.1.0.0/16 → local
0.0.0.0/0   → Internet Gateway
```

Associated the route table with `xfusion-pub-subnet`.

### 4. Created NAT Security Group

Created custom security group:

```text
xfusion-nat-sg
sg-07542a5de38dec1d7
```

Inbound traffic was allowed from:

```text
10.1.1.0/24
```

Outbound traffic was allowed to:

```text
0.0.0.0/0
```

### 5. Launched Amazon Linux 2023 NAT Instance

Launched:

```text
Name: xfusion-nat-instance
Instance Type: t3.micro
AMI: Amazon Linux 2023
Subnet: xfusion-pub-subnet
```

A public IP was assigned because the NAT instance needs direct internet access through the Internet Gateway.

### 6. Disabled Source/Destination Check

NAT instances must have source/destination checking disabled.

Verified:

```text
SourceDestCheck = False
```

### 7. Configured iptables

Amazon Linux 2023 does not have the required iptables service installed by default.

Installed:

```bash
dnf install -y iptables-services
```

Enabled IPv4 forwarding:

```text
net.ipv4.ip_forward = 1
```

Configured forwarding rules:

```text
10.1.1.0/24 → FORWARD
```

Configured NAT/MASQUERADE:

```text
10.1.1.0/24 → ens5 → MASQUERADE
```

### Important Troubleshooting

Initially, the MASQUERADE rule was configured for `eth0`.

However, the Amazon Linux 2023 NAT instance used:

```text
ens5
```

The `eth0` rule had a packet count of `0`, indicating that it was not processing the private subnet traffic.

After checking:

```bash
ip route
ip -br addr
```

the correct interface was identified as `ens5`.

The MASQUERADE rule was corrected to:

```bash
iptables -t nat -A POSTROUTING -s 10.1.1.0/24 -o ens5 -j MASQUERADE
```

The rules were then saved and the iptables service restarted.

### 8. Configured Private Route Table

Private route table:

```text
rtb-0f1199cf07000bb7a
```

Added:

```text
0.0.0.0/0 → NAT Instance
```

Final routing:

```text
10.1.0.0/16 → local
0.0.0.0/0   → i-0dddf73974a34288d
```

## Verification

Verified the S3 object:

```bash
aws s3api head-object \
  --bucket xfusion-nat-32213 \
  --key xfusion-test.txt
```

Result:

```text
ContentLength: 18
ContentType: text/plain
ServerSideEncryption: AES256
```

This confirmed that the private EC2 successfully accessed the internet through the NAT Instance and uploaded the test file to S3.

## Key Learnings

* A private subnet cannot directly access the internet through an Internet Gateway.
* A NAT Instance can provide outbound internet access for private instances.
* NAT Instances require **Source/Destination Check to be disabled**.
* IPv4 forwarding must be enabled.
* `iptables` can perform NAT using MASQUERADE.
* The NAT instance itself must be in a public subnet with a route to an Internet Gateway.
* The private subnet's default route must point to the NAT instance.
* Network interface names should not be assumed. Amazon Linux 2023 used `ens5` in this lab instead of `eth0`.
* Packet counters in `iptables` are useful for troubleshooting whether traffic is actually reaching a rule.

## Status

**Day 30 — Completed Successfully ✅**
