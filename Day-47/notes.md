# Day 47 - AWS Priority Queuing with SQS, SNS and Lambda

## Objective
Implemented a priority-based message processing system using Amazon SQS, SNS, Lambda, IAM, and CloudFormation.

## Resources Created

- CloudFormation Stack: `nautilus-priority-stack`
- High Priority Queue: `nautilus-High-Priority-Queue`
- Low Priority Queue: `nautilus-Low-Priority-Queue`
- SNS Topic: `nautilus-Priority-Queues-Topic`
- Lambda Function: `nautilus-priorities-queue-function`
- IAM Role: `lambda_execution_role`

## SNS Configuration

The SNS topic has two SQS subscriptions:

- High-priority subscription filters `priority = high`
- Low-priority subscription filters `priority = low`
- Filter policy scope: `MessageAttributes`

## Lambda Configuration

- Runtime: Python 3.12
- Memory: 128 MB
- Timeout: 10 seconds
- Handler: `index.lambda_handler`

The Lambda checks the high-priority queue first. It only checks the low-priority queue when no high-priority message is available.

## Testing

Published four messages through SNS:

- High Priority message 1
- High Priority message 2
- Low Priority message 1
- Low Priority message 2

SNS correctly routed:

- 2 messages to the high-priority queue
- 2 messages to the low-priority queue

Lambda processing was verified in this priority order:

1. High Priority message 2
2. High Priority message 1
3. Low Priority message 1
4. Low Priority message 2

Both high-priority messages were processed before any low-priority message.

## Final Verification

High-priority queue:

- Messages: 0
- Messages not visible: 0

Low-priority queue:

- Messages: 0
- Messages not visible: 0

## Result

Day 47 priority queuing implementation was successfully deployed through CloudFormation and verified end-to-end.