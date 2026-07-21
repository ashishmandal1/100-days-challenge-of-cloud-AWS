# AWS Day 17: Create an IAM Group

## Task

Create an IAM group named `iamgroup_yousuf`.

## Requirements

* **IAM Group Name:** `iamgroup_yousuf`
* **Region:** `us-east-1`

## Steps Performed

1. Logged in to the AWS Management Console.
2. Opened the **IAM** service.
3. Navigated to **User groups**.
4. Selected **Create group**.
5. Entered the group name:

```text
iamgroup_yousuf
```

6. Created the IAM group successfully.

## Verification

The IAM group `iamgroup_yousuf` was successfully created and is visible under:

```text
IAM → User groups
```

## Key Concepts Learned

* IAM stands for **Identity and Access Management**.
* IAM groups are collections of IAM users.
* Permissions can be assigned to a group instead of assigning the same policies individually to multiple users.
* Users added to the group inherit the permissions attached to the group.
* IAM is a global AWS service, so the selected AWS region does not affect IAM resources.

## Result

✅ Successfully created the IAM group `iamgroup_yousuf`.
