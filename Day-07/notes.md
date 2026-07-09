# Day 07 - Change EC2 Instance Type

## Objective
Change the EC2 instance type from t2.micro to t2.nano.

## Steps Performed
1. Logged in to AWS Console.
2. Opened EC2 Dashboard.
3. Located the `devops-ec2` instance.
4. Waited until status checks were complete.
5. Stopped the instance.
6. Changed the instance type from `t2.micro` to `t2.nano`.
7. Started the instance.
8. Verified the instance was running with the new instance type.

## Outcome
Successfully changed the EC2 instance type from `t2.micro` to `t2.nano`.

## Key Learnings
- An EC2 instance must be stopped before changing its instance type.
- Instance type determines CPU, memory, and networking capacity.
- Status checks should pass before modifying an EC2 instance.
- After the change, the instance can be restarted with the new configuration.

## Status

✅ Day 5 Completed