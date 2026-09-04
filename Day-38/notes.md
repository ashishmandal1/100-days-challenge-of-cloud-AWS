# Day 38 - AWS ECR & ECS Fargate

* Created private ECR repository `xfusion-ecr` in `us-east-1`.
* Built Docker image from `/root/pyapp` using Nginx Alpine.
* Tagged image as `latest` and pushed it to ECR.
* Created ECS Fargate cluster `xfusion-cluster`.
* Created `ecsTaskExecutionRole` with ECR execution permissions.
* Registered Fargate task definition `xfusion-taskdefinition:2`.
* Configured task with 256 CPU, 512 MiB memory, and port 80.
* Created ECS service `xfusion-service` with desired count 1.
* Verified the Fargate task reached `RUNNING` state successfully.
