# Day 34 - AWS Lambda Function

## Objective

Create and deploy an AWS Lambda function using the AWS CLI with a Python deployment package and an existing IAM execution role.

## Resources Created

* Lambda Function: `devops-lambda-cli`
* Runtime: Python 3.14
* Handler: `lambda_function.lambda_handler`
* IAM Role: `lambda_execution_role`
* Deployment Package: `function.zip`
* Region: `us-east-1`

## Python Code

```python
import json

def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Welcome to KKE AWS Labs!'
    }
```

## Steps Performed

1. Created a working directory:
   `~/day34-lambda`

2. Created `lambda_function.py` containing the Lambda handler.

3. Packaged the Python file:
   `zip function.zip lambda_function.py`

4. Retrieved the ARN of the existing IAM role:
   `lambda_execution_role`

5. Created the Lambda function using:
   `aws lambda create-function`

6. Waited for the Lambda function to become active.

7. Verified the function configuration using:
   `aws lambda get-function`

8. Invoked the function using:
   `aws lambda invoke`

## Verification

The Lambda function reached the `Active` state.

Invocation response:

```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}
```

## Key AWS CLI Commands

```bash
aws iam get-role \
  --role-name lambda_execution_role \
  --query 'Role.Arn' \
  --output text
```

```bash
aws lambda create-function \
  --function-name devops-lambda-cli \
  --runtime python3.14 \
  --role arn:aws:iam::977071274244:role/lambda_execution_role \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://function.zip
```

```bash
aws lambda wait function-active-v2 \
  --function-name devops-lambda-cli
```

```bash
aws lambda invoke \
  --function-name devops-lambda-cli \
  --payload '{}' \
  response.json
```

## What I Learned

* How to create a Python Lambda deployment package.
* How to deploy a ZIP-based Lambda function using AWS CLI.
* How IAM execution roles are associated with Lambda functions.
* How to verify Lambda configuration and state.
* How to invoke and test a Lambda function from the AWS CLI.
