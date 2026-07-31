# Day 25 – AWS CloudWatch Alarm for EC2 CPU Monitoring

## Objective

The objective of this lab was to launch an Amazon EC2 instance and configure an Amazon CloudWatch alarm to monitor its CPU utilization. The alarm was set to trigger when CPU utilization reached or exceeded 90% for one consecutive 5-minute period and send a notification using an existing Amazon SNS topic.

---

## AWS Services Used

- Amazon EC2
- Amazon CloudWatch
- Amazon SNS

---

## What is Amazon CloudWatch?

Amazon CloudWatch is a monitoring and observability service that collects metrics, logs, and events from AWS resources. It helps monitor resource health, application performance, and system activity in real time.

CloudWatch can monitor:

- EC2 Instances
- EBS Volumes
- RDS Databases
- Lambda Functions
- Auto Scaling Groups
- Custom Metrics

---

## What is a CloudWatch Alarm?

A CloudWatch Alarm monitors a CloudWatch metric and automatically changes its state when a configured threshold is reached.

Alarm States:

### OK
The monitored metric is within the configured threshold.

### ALARM
The monitored metric has crossed the configured threshold.

### INSUFFICIENT_DATA
CloudWatch does not yet have enough metric data to determine the alarm state. This is normal immediately after creating an alarm.

---

## EC2 Instance Details

**Instance Name:** datacenter-ec2

**Operating System:** Ubuntu Server

**Region:** us-east-1

---

## CloudWatch Alarm Configuration

**Alarm Name:** datacenter-alarm

**Metric:** CPUUtilization

**Namespace:** AWS/EC2

**Statistic:** Average

**Period:** 5 Minutes

**Threshold:** CPUUtilization >= 90%

**Evaluation Period:** 1 consecutive 5-minute period

**Alarm Action:** Send notification to SNS Topic

**SNS Topic:** datacenter-sns-topic

---

## Steps Performed

### Step 1
Logged in to the AWS Management Console.

### Step 2
Opened the EC2 Dashboard.

### Step 3
Launched a new Ubuntu EC2 instance named **datacenter-ec2**.

### Step 4
Verified that the instance entered the **Running** state.

### Step 5
Opened Amazon CloudWatch.

### Step 6
Navigated to:

CloudWatch → Alarms → Create Alarm

### Step 7
Selected the metric:

EC2 → Per-Instance Metrics → CPUUtilization

### Step 8
Configured the metric:

- Statistic: Average
- Period: 5 Minutes
- Threshold: Greater than or equal to 90%

### Step 9
Configured Alarm Actions:

- Alarm State: In Alarm
- Selected existing SNS Topic
- SNS Topic: datacenter-sns-topic

### Step 10
Provided the alarm name:

datacenter-alarm

Reviewed the configuration and created the alarm.

---

## Why Monitor CPU Utilization?

Monitoring CPU utilization helps to:

- Detect high server load
- Identify performance bottlenecks
- Monitor application health
- Detect unusual traffic spikes
- Prevent resource exhaustion
- Improve system reliability

---

## Why Use Amazon SNS?

Amazon Simple Notification Service (SNS) sends notifications whenever a CloudWatch alarm changes state.

SNS supports notifications through:

- Email
- SMS
- Lambda
- HTTP/HTTPS
- Amazon SQS
- Mobile Push Notifications

---

## Benefits of CloudWatch Alarms

- Continuous monitoring
- Automatic notifications
- Faster issue detection
- Reduced downtime
- Improved operational reliability
- Supports automation with AWS services

---

## Key Concepts Learned

- Launching an EC2 instance
- Monitoring EC2 CPU utilization
- Creating CloudWatch alarms
- Configuring alarm thresholds
- Understanding alarm states
- Integrating CloudWatch with Amazon SNS
- Monitoring AWS resources in real time

---

## Outcome

Successfully completed the following tasks:

- Created an EC2 instance named **datacenter-ec2**
- Created a CloudWatch alarm named **datacenter-alarm**
- Monitored the **CPUUtilization** metric
- Configured the alarm threshold to **90%**
- Configured notifications using the existing **datacenter-sns-topic**
- Learned how CloudWatch and SNS work together to provide monitoring and alerting for AWS resources

---

## Conclusion

In this lab, an Amazon EC2 instance was launched and monitored using Amazon CloudWatch. A CloudWatch alarm was configured to monitor CPU utilization and send notifications through Amazon SNS whenever CPU usage reached or exceeded 90% for one consecutive 5-minute period. This lab demonstrated how AWS monitoring and alerting services help maintain application performance, improve availability, and enable proactive infrastructure management.