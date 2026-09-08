# Day 42 — DynamoDB Tasks

## Objective

Create a DynamoDB table to store To-Do application tasks and verify task statuses.

## AWS Service

* DynamoDB
* Region: `us-east-1`

## Table Configuration

| Setting       | Value            |
| ------------- | ---------------- |
| Table Name    | `nautilus-tasks` |
| Partition Key | `taskId`         |
| Key Type      | String           |

## Tasks Added

### Task 1

* **taskId:** `1`
* **description:** `Learn DynamoDB`
* **status:** `completed`

### Task 2

* **taskId:** `2`
* **description:** `Build To-Do App`
* **status:** `in-progress`

## Verification

Verified the DynamoDB table using AWS CLI:

```bash
aws dynamodb scan \
  --table-name nautilus-tasks \
  --region us-east-1 \
  --query 'Items[*].{taskId:taskId.S,description:description.S,status:status.S}' \
  --output table
```

### Verified Result

```text
----------------------------------------------
|                    Scan                    |
+------------------+---------------+---------+
|    description   |    status     | taskId  |
+------------------+---------------+---------+
|  Build To-Do App |  in-progress  |  2      |
|  Learn DynamoDB  |  completed    |  1      |
+------------------+---------------+---------+
```

## Final Status

* [x] DynamoDB table created successfully
* [x] Correct partition key configured
* [x] Task 1 inserted
* [x] Task 2 inserted
* [x] Task 1 status verified
* [x] Task 2 status verified
* [x] Final AWS CLI verification completed

**Day 37 completed successfully.**
