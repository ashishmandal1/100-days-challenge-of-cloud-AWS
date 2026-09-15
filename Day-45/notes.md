# Day 45 — NAT Gateway for Private EC2

## Objective

Enable internet access for an EC2 instance in a private subnet by configuring a NAT Gateway in a public subnet.

## Resources Created

* VPC: `nautilus-priv-vpc`
* VPC ID: `vpc-0d3b6bf401da846e4`
* Private subnet: `nautilus-priv-subnet`
* Private subnet ID: `subnet-00a533dc7fa27418f`
* Public subnet: `nautilus-pub-subnet`
* Public subnet ID: `subnet-0c06fd36577b21182`
* Internet Gateway: `nautilus-igw`
* Internet Gateway ID: `igw-09d4615ee46dc9c3b`
* Public route table: `nautilus-pub-rt`
* Public route table ID: `rtb-03c1d77bcb1888706`
* Elastic IP: `32.194.73.161`
* NAT Gateway: `nautilus-natgw`
* NAT Gateway ID: `nat-064afb0a425f3be04`
* Private route table: `nautilus-priv-rt`
* Private route table ID: `rtb-0d98264db3dfe5a03`

## Network Configuration

### Public subnet

CIDR:
`10.1.2.0/24`

Route:
`0.0.0.0/0 → Internet Gateway`

### Private subnet

CIDR:
`10.1.1.0/24`

Dedicated route table:
`nautilus-priv-rt`

Route:
`0.0.0.0/0 → nautilus-natgw`

The private subnet was explicitly associated with the dedicated private route table instead of modifying the VPC Main route table.

## Verification

NAT Gateway status:

`available`

S3 bucket:

`nautilus-nat-694811873`

Verification command:

```bash
aws s3 ls s3://nautilus-nat-694811873
```

Result:

```text
2026-09-15 11:17:11          0 nautilus-test.txt
```

The appearance of `nautilus-test.txt` confirms that the private EC2 instance successfully reached the internet and uploaded the test file to S3 through the NAT Gateway.

## Result

**Day 45 completed successfully.**
