# Day 20 - Create IAM Role

## Task

Create an IAM role with the following requirements:

* **Role Name:** `iamrole_mark`
* **Trusted Entity:** AWS Service
* **Use Case:** EC2
* **Attached Policy:** `iampolicy_mark`
* **Region:** `us-east-1`

## What is an IAM Role?

An IAM role is an identity that provides temporary permissions to AWS services, applications, or users. Unlike IAM users, IAM roles do not have permanent login credentials.

In this task, an IAM role was created for the EC2 service. The role allows EC2 instances to assume the role and use the permissions provided by the attached IAM policy.

## Steps Performed

1. Opened the AWS Management Console.
2. Navigated to the IAM service.
3. Selected **Roles**.
4. Clicked **Create role**.
5. Selected **AWS service** as the trusted entity type.
6. Selected **EC2** as the use case.
7. Attached the policy:

   ```text
   iampolicy_mark
   ```
8. Set the role name:

   ```text
   iamrole_mark
   ```
9. Created the IAM role.

## Result

Successfully created the IAM role:

```text
iamrole_mark
```

The role was configured for the EC2 service with the following policy attached:

```text
iampolicy_mark
```

## Key Learnings

* IAM roles provide permissions without requiring permanent credentials.
* EC2 instances can assume IAM roles.
* Trust policies define who or what can assume a role.
* Permission policies define what actions the role can perform.
* IAM roles are a secure way to grant AWS services access to other AWS resources.

## AWS Service

* **Service:** AWS Identity and Access Management (IAM)
* **Resource:** IAM Role
* **Use Case:** EC2
* **Region:** us-east-1
