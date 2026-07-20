---
title : "Monitoring the System with Amazon CloudWatch"
date : 2025-07-15
weight : 17
chapter : true
pre : " <b> 5.17. </b> "
---

# Monitoring the System with Amazon CloudWatch

## Introduction

Amazon CloudWatch is AWS's monitoring and logging service.

In the **Smart Campus Guardian – AI Campus Incident Detection Platform** project, Amazon CloudWatch is used to monitor the entire system, including logs, metrics, dashboards, and alarms.

CloudWatch enables the operations team to quickly detect issues, monitor the performance of the AI workflow, and receive alerts whenever problems occur.

---

## Objectives

After completing this chapter, you will be able to:

+ Monitor logs across the entire system.
+ Monitor metrics.
+ Create a dashboard.
+ Create alarms for system errors.
+ Verify the AI workflow.

---

# Amazon CloudWatch Architecture

Amazon CloudWatch is connected to all services in the system to collect logs and metrics.

![CloudWatch Architecture](/images/5-Workshop/5.17/5.17.1/cloudwatch-architecture.png)

---

## Architecture Overview

CloudWatch collects logs and metrics from the following AWS services:

+ Amazon API Gateway
+ AWS Lambda
+ Amazon EventBridge
+ AWS Step Functions
+ Amazon Rekognition
+ Amazon Bedrock
+ Amazon DynamoDB
+ Amazon SNS
+ Amazon SES

After collecting the data, CloudWatch will:

1. Record logs for each service.
2. Collect metrics.
3. Display dashboards.
4. Trigger alarms when errors are detected.

CloudWatch serves as the centralized monitoring service for the entire Smart Campus Guardian system.

---

# Deployment Steps

## Step 1: Open Amazon CloudWatch

Sign in to the **AWS Management Console**.

Search for:

```
Amazon CloudWatch
```

---

## Step 2: Review Logs

Select:

```
Logs

↓

Log Groups
```

You will see log groups for:

```
AWS Lambda

API Gateway

Step Functions

EventBridge
```

![Log Groups](/images/5-Workshop/5.17/5.17.2/log-groups.png)

---

## Step 3: Create a Dashboard

Select:

```
Dashboards

↓

Create Dashboard
```

Dashboard Name

```
SmartCampusGuardianDashboard
```

---

## Step 4: Add Widgets

Add the following metrics:

```
Lambda Invocations

Lambda Errors

API Gateway Requests

Step Functions Executions

DynamoDB Read Capacity

SNS Messages Published

SES Send Email

EventBridge Invocations
```

The dashboard will display the operational status of the entire system in real time.

---

## Step 5: Create an Alarm

Select:

```
Alarms

↓

Create Alarm
```

Metric

```
Lambda Errors
```

Condition

```
Greater than 5
```

Evaluation Period

```
5 Minutes
```

Action

```
Publish to SNS Topic

SmartCampusAlertTopic
```

When the number of Lambda errors exceeds the threshold, CloudWatch automatically sends an alert.

---

## Step 6: Verify

Upload an image:

```
fire.jpg
```

After the AI workflow completes:

```
CloudWatch

↓

Dashboard

↓

Logs

↓

Metrics

↓

Alarm
```

Verify that the dashboard is updated with the latest data and that the AI workflow logs are recorded.

---

## Best Practices

✔ Configure log retention.

✔ Monitor critical metrics.

✔ Create separate dashboards for each environment.

✔ Configure alarms for Lambda, API Gateway, and Step Functions.

✔ Use Amazon SNS to receive notifications.

---

## Verification

After completing this chapter, you will have:

+ A system monitoring dashboard.

+ Logs for all AWS services.

+ Active alarms.

+ Real-time metrics.

---

## Summary

In this chapter, you successfully deployed Amazon CloudWatch to monitor the entire **Smart Campus Guardian** system.

CloudWatch helps monitor the performance of the AI workflow, collect logs, visualize operational data through dashboards, and send alerts whenever system issues occur, enabling the operations team to manage the platform more effectively.

In the next chapter, we will deploy the **ReactJS frontend** on Amazon S3 and Amazon CloudFront to complete the system.