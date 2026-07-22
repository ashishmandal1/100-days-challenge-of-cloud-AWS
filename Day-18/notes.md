# AWS Day 18: Create IAM Read-Only EC2 Policy

## Task

Create an IAM policy named `iampolicy_rose` that provides read-only access to EC2 resources, allowing users to view:

* EC2 instances
* Amazon Machine Images (AMIs)
* EBS snapshots

The resource was created according to the lab requirements in the `us-east-1` region.

## Policy Name

```text
iampolicy_rose
```

## IAM Policy JSON

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "ec2:DescribeImages",
                "ec2:DescribeSnapshots"
            ],
            "Resource": "*"
        }
    ]
}
```

## Permissions Explained

| Permission              | Purpose                      |
| ----------------------- | ---------------------------- |
| `ec2:DescribeInstances` | Allows viewing EC2 instances |
| `ec2:DescribeImages`    | Allows viewing AMIs          |
| `ec2:DescribeSnapshots` | Allows viewing EBS snapshots |

## Key Learning

IAM policies define what actions users, groups, or roles are allowed to perform. The `Describe` actions in Amazon EC2 are read-only actions and do not allow users to create, modify, or delete resources.

## Status

✅ IAM policy `iampolicy_rose` created successfully.
