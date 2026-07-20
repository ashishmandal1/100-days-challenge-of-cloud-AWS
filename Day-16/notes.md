# Day 16 - Create IAM User

## Objective
Create an IAM user named `iamuser_ravi`.

## Service Used
- AWS Identity and Access Management (IAM)

## Steps Performed
1. Logged in to the AWS Management Console.
2. Opened the IAM service.
3. Navigated to **Users**.
4. Clicked **Create user**.
5. Entered the username `iamuser_ravi`.
6. Left console access disabled.
7. Created the IAM user successfully.

## AWS CLI Command

```bash
aws iam create-user --user-name iamuser_ravi
```

## Verification

```bash
aws iam get-user --user-name iamuser_ravi
```

## Key Learning
- IAM users represent identities for people or applications.
- IAM is a global AWS service used for authentication and authorization.
- Users can later be assigned groups, roles, and policies to control permissions.