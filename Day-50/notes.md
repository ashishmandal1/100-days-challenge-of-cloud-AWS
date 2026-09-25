# Day 50 – Expand EC2 EBS Volume

## 🎯 Task

Increase the root EBS volume attached to the `devops-ec2` instance from **8 GiB to 12 GiB** and ensure that the root filesystem `/` also reflects the increased capacity.

## ☁️ AWS Resources

* **Instance Name:** `devops-ec2`
* **Instance ID:** `i-0ecca68ec996a27b1`
* **Public IP:** `34.228.140.255`
* **Volume ID:** `vol-0bed5a7e38498b348`
* **Device:** `/dev/xvda`
* **Root Partition:** `/dev/xvda1`
* **Region:** `us-east-1`
* **SSH Key:** `/root/devops-keypair.pem`

## 🔧 Steps Performed

### 1. Identify the EC2 instance and attached volume

```bash
aws ec2 describe-instances \
  --filters "Name=tag:Name,Values=devops-ec2" \
  --query 'Reservations[0].Instances[0].[InstanceId,PublicIpAddress,State.Name,BlockDeviceMappings]' \
  --output table \
  --region us-east-1
```

The instance was found in the `running` state with the root device `/dev/xvda`.

### 2. Check the current EBS volume

```bash
aws ec2 describe-volumes \
  --volume-ids vol-0bed5a7e38498b348 \
  --query 'Volumes[0].[VolumeId,Size,State,Attachments[0].Device]' \
  --output table \
  --region us-east-1
```

Initial size:

```text
8 GiB
```

### 3. Increase the EBS volume to 12 GiB

```bash
aws ec2 modify-volume \
  --volume-id vol-0bed5a7e38498b348 \
  --size 12 \
  --region us-east-1
```

The modification changed the target size from **8 GiB → 12 GiB**.

### 4. Verify the EBS volume

```bash
aws ec2 describe-volumes \
  --volume-ids vol-0bed5a7e38498b348 \
  --query 'Volumes[0].[VolumeId,Size,State,Attachments[0].Device]' \
  --output table \
  --region us-east-1
```

The volume was verified as:

```text
12 GiB
in-use
/dev/xvda
```

### 5. Connect to the EC2 instance

```bash
chmod 400 /root/devops-keypair.pem

ssh -i /root/devops-keypair.pem \
  -o StrictHostKeyChecking=no \
  ec2-user@34.228.140.255
```

### 6. Check the disk and partition

```bash
lsblk
```

Initially, the disk was 12 GiB but the root partition was still 8 GiB:

```text
xvda      12G
└─xvda1    8G /
```

### 7. Expand the root partition

```bash
sudo growpart /dev/xvda 1
```

The root partition `/dev/xvda1` was successfully expanded to use the available disk space.

### 8. Expand the XFS filesystem

The root filesystem was XFS, so it was expanded with:

```bash
sudo xfs_growfs /
```

### 9. Verify the root filesystem

```bash
df -h /
```

Result:

```text
/dev/xvda1   12G   1.6G   11G   13%   /
```

The exact validation command also returned:

```bash
df -h / | grep /dev/xvda1 | awk '{print $2}'
```

Output:

```text
12G
```

## 🧪 KodeKloud Automated Validation

After completing the configuration, the provided KodeKloud test was executed:

```bash
cd /usr/share
pytest -v test.py
```

Result:

```text
test.py::test_volume_expansion PASSED [100%]

1 passed in 5.42s
```

## ✅ Final Result

* EBS volume expanded from **8 GiB → 12 GiB**
* Root partition `/dev/xvda1` expanded to **12 GiB**
* XFS root filesystem expanded successfully
* `/` verified at **12 GiB**
* KodeKloud automated validation **PASSED**

### 🏆 Day 50 Status

**Day 50 — Completed and Verified ✅**

**50 / 100 Days Completed 🚀**
