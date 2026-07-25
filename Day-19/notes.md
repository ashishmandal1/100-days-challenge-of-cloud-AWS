# AWS Day 19 – Attach IAM Policy to IAM User

## Task

Attach the existing IAM policy `iampolicy_jim` to the existing IAM user `iamuser_jim`.

## AWS Region

* `us-east-1`

## Existing Resources

* IAM User: `iamuser_jim`
* IAM Policy: `iampolicy_jim`

## Steps Performed

1. Logged in to the AWS Management Console using the provided lab credentials.
2. Opened the IAM service.
3. Navigated to **Users**.
4. Selected the user `iamuser_jim`.
5. Opened the **Permissions** tab.
6. Chose **Add permissions**.
7. Selected **Attach policies directly**.
8. Searched for and selected `iampolicy_jim`.
9. Attached the policy to the user.

## Result

The IAM policy `iampolicy_jim` was successfully attached to the IAM user `iamuser_jim`.

## Key Concept

IAM policies define permissions in AWS. Attaching a policy to a user grants that user the permissions specified in the policy.

## Important

IAM is a global AWS service, but the lab instructions specified using the `us-east-1` region.
