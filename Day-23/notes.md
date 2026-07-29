# Day 23 - AWS S3 Data Migration Using AWS CLI

## Task

Create a new private S3 bucket and migrate all data from an existing S3 bucket using the AWS CLI while ensuring complete data consistency.

## Services Used

- Amazon S3
- AWS CLI

## Steps Performed

- Verified AWS CLI credentials.
- Created a new private S3 bucket.
- Synced all objects from the source bucket to the destination bucket.
- Verified both buckets contained the same number of objects.
- Confirmed successful data migration.

## Commands Used

```bash
aws sts get-caller-identity

aws s3 mb s3://datacenter-sync-19241 --region us-east-1

aws s3 sync s3://datacenter-s3-23451 s3://datacenter-sync-19241

aws s3 ls s3://datacenter-s3-23451 --recursive

aws s3 ls s3://datacenter-sync-19241 --recursive

aws s3 ls s3://datacenter-s3-23451 --recursive | wc -l

aws s3 ls s3://datacenter-sync-19241 --recursive | wc -l
```

## Outcome

Successfully migrated all objects from the source S3 bucket to the destination S3 bucket using the AWS CLI. Verified that both buckets contained **3945** objects, confirming a successful migration.