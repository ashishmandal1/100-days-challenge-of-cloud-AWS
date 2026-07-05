# Day 4: AWS S3 Bucket Versioning

## Task

Enabled versioning for the S3 bucket named `nautilus-s3-4381`.

## Description

Implemented data protection and recovery measures by enabling versioning on the S3 bucket. Versioning helps preserve, retrieve, and restore every version of every object stored in the bucket, protecting against accidental deletion or unintended overwrites.

## Configuration

* Region: `us-east-1`
* Bucket Name: `nautilus-s3-4381`
* Versioning Status: `Enabled`

## Steps Performed

* Logged in to the AWS Management Console using the provided credentials.
* Verified that the selected region was `us-east-1`.
* Opened the Amazon S3 service.
* Selected the bucket `nautilus-s3-4381`.
* Navigated to the **Properties** tab.
* Edited the **Bucket Versioning** settings.
* Enabled versioning and saved the changes.
* Confirmed that the bucket status changed to **Enabled**.

## AWS CLI Verification

```bash
# Verify bucket versioning status
aws s3api get-bucket-versioning \
  --bucket nautilus-s3-4381 \
  --region us-east-1
```

Expected output:

```json
{
  "Status": "Enabled"
}
```

## What I Learned

* S3 Versioning maintains multiple versions of an object within the same bucket.
* Accidental deletions can be reversed using previous object versions.
* Overwritten files remain recoverable because older versions are preserved.
* Versioning is a key component of backup and disaster recovery strategies.
* Once enabled, versioning can only be suspended, not permanently disabled.
* Combining versioning with lifecycle policies helps optimize storage costs.

## Status

✅ Day 4 Completed
