# Day 46 – S3 to S3 File Copy Using Lambda with DynamoDB Logging

## Resources

* Public S3 Bucket: `xfusion-public-773839579`
* Private S3 Bucket: `xfusion-private-773839579`
* Lambda Function: `xfusion-copyfunction`
* IAM Role: `lambda_execution_role`
* DynamoDB Table: `xfusion-S3CopyLogs`
* Region: `us-east-1`

## Configuration

* Public S3 bucket configured to allow public object access
* Private S3 bucket configured to block public access
* Lambda Runtime: Python 3.12
* Lambda Handler: `lambda-function.lambda_handler`
* Lambda Memory: 128 MB
* Lambda Timeout: 10 seconds
* S3 `ObjectCreated` trigger configured
* Lambda permission to read from the public bucket
* Lambda permission to write to the private bucket
* Lambda permission to write logs to DynamoDB
* CloudWatch logging enabled

## Lambda Workflow

1. A file is uploaded to the public S3 bucket.
2. S3 automatically triggers the Lambda function.
3. Lambda reads the source bucket and object key from the S3 event.
4. Lambda copies the uploaded file to the private S3 bucket.
5. Lambda creates a DynamoDB log entry containing:

   * `LogID`
   * `SourceBucket`
   * `DestinationBucket`
   * `ObjectKey`
   * `Timestamp`
   * `Status`

## DynamoDB

* Table: `xfusion-S3CopyLogs`
* Partition Key: `LogID`
* Key Type: String
* Billing Mode: `PAY_PER_REQUEST`
* Table Status: `ACTIVE`

## Testing

Test file:

* `/root/sample.zip`

Uploaded to:

* `s3://xfusion-public-773839579/sample.zip`

Verified in private bucket:

* `s3://xfusion-private-773839579/sample.zip`
* Size: 164 bytes
* Content-Type: `application/zip`

## DynamoDB Log Verification

* SourceBucket: `xfusion-public-773839579`
* DestinationBucket: `xfusion-private-773839579`
* ObjectKey: `sample.zip`
* Status: `Success`

## Result

The complete S3 → Lambda → private S3 + DynamoDB logging workflow was successfully tested and verified end-to-end.

**Day 46: COMPLETED ✅**
