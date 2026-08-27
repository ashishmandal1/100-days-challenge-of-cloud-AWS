# Day 32 — AWS RDS Snapshot and Restore

## Overview

Today I worked with **Amazon RDS** to create a database snapshot of an existing MySQL RDS instance and restore that snapshot to a new RDS instance.

## Task

* Existing RDS instance: `xfusion-rds`
* Snapshot name: `xfusion-snapshot`
* Restored RDS instance: `xfusion-snapshot-restore`
* Required instance class: `db.t3.micro`
* Region: `us-east-1`

## Steps Performed

### 1. Checked the Existing RDS Instance

Verified that `xfusion-rds` was in the `available` state before creating the snapshot.

```bash
aws rds describe-db-instances \
  --db-instance-identifier xfusion-rds \
  --region us-east-1 \
  --query 'DBInstances[0].[DBInstanceIdentifier,DBInstanceClass,DBInstanceStatus,Engine]' \
  --output table
```

### 2. Created an RDS Snapshot

Created a manual snapshot named `xfusion-snapshot`.

```bash
aws rds create-db-snapshot \
  --db-instance-identifier xfusion-rds \
  --db-snapshot-identifier xfusion-snapshot \
  --region us-east-1
```

Waited until the snapshot status became `available`.

```bash
aws rds wait db-snapshot-available \
  --db-snapshot-identifier xfusion-snapshot \
  --region us-east-1
```

### 3. Restored the Snapshot

Restored the snapshot to a new RDS instance named `xfusion-snapshot-restore` using the required `db.t3.micro` instance class.

```bash
aws rds restore-db-instance-from-db-snapshot \
  --db-instance-identifier xfusion-snapshot-restore \
  --db-snapshot-identifier xfusion-snapshot \
  --db-instance-class db.t3.micro \
  --region us-east-1
```

### 4. Verified the Restored Instance

Checked the restored RDS instance until it reached the `available` state.

```bash
aws rds describe-db-instances \
  --db-instance-identifier xfusion-snapshot-restore \
  --region us-east-1 \
  --query 'DBInstances[0].[DBInstanceIdentifier,DBInstanceClass,DBInstanceStatus,Engine]' \
  --output table
```

Final result:

```text
xfusion-snapshot-restore
db.t3.micro
available
mysql
```

## Key Learnings

* Learned how to create manual snapshots of Amazon RDS instances.
* Learned how to restore an RDS snapshot into a new database instance.
* Learned how to use AWS CLI wait commands for RDS resources.
* Learned that RDS operations can pass through intermediate states such as `creating`, `configuring-enhanced-monitoring`, `backing-up`, and `modifying` before becoming `available`.
* Practiced verifying RDS instance configuration and status using AWS CLI.
* Reinforced the importance of working in the required AWS region.

## Final Status

**Day 32 — Completed successfully ✅**

RDS snapshot creation and restoration were completed successfully in `us-east-1`.
