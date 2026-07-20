---
title : "Deploy AWS Step Functions"
date : 2025-07-15
weight : 11
chapter : true
pre : " <b> 5.11. </b> "
---

# Deploy AWS Step Functions

## Introduction

AWS Step Functions is AWS's workflow orchestration service that enables you to build complex workflows by connecting multiple AWS services into a State Machine.

In the **Smart Campus Guardian – AI Campus Incident Detection Platform** project, AWS Step Functions orchestrates the entire AI analysis workflow after a camera uploads an image to Amazon S3.

Instead of allowing Lambda functions to invoke one another directly, Step Functions manages the entire workflow, making the system easier to scale, monitor, and automatically retry when failures occur.

---

## Objectives

After completing this chapter, you will be able to:

+ Create a State Machine.
+ Build the AI Workflow.
+ Configure a Retry Policy.
+ Configure Error Handling.
+ Connect the AI Lambda function with Amazon Rekognition and Amazon Bedrock.

---

# AWS Step Functions Architecture

AWS Step Functions serves as the AI Workflow orchestrator for Smart Campus Guardian.

After being triggered by EventBridge, Step Functions sequentially invokes the AI Lambda function, Amazon Rekognition, Amazon Bedrock, and the storage and notification services.

![Step Functions Architecture](/images/5-Workshop/5.11/5.11.1/stepfunctions-architecture.png)

---

## Architecture Overview

The workflow operates as follows:

1. Amazon EventBridge triggers the State Machine.
2. AWS Step Functions invokes the AI Lambda function.
3. The AI Lambda function retrieves the image from Amazon S3.
4. The AI Lambda function calls Amazon Rekognition to detect objects.
5. The detection results are sent to Amazon Bedrock for risk analysis.
6. The AI Lambda function stores the incident in Amazon DynamoDB.
7. Amazon SNS sends a notification.
8. Amazon SES sends an email.
9. Amazon CloudWatch records logs and metrics.

Step Functions provides a visual workflow, simplifies maintenance, and automatically retries failed tasks when necessary.

---

# Deployment Steps

## Step 1: Open AWS Step Functions

Sign in to the AWS Management Console.

Search for:

```
AWS Step Functions
```

Choose:

```
Create State Machine
```

---

## Step 2: Create a State Machine

Workflow Type

```
Standard
```

State Machine Name

```
SmartCampusAIWorkflow
```

![Create State Machine](/images/5-Workshop/5.11/5.11.2/state-machine.png)

---

## Step 3: Design the AI Workflow

The State Machine consists of the following steps:

```
Receive Image

↓

Lambda AI

↓

Amazon Rekognition

↓

Amazon Bedrock

↓

Lambda AI

↓

Amazon DynamoDB

↓

Amazon SNS

↓

Amazon SES

↓

Success
```

---

## Step 4: Configure Retry

Retry Count

```
3
```

Interval

```
5 Seconds
```

Backoff Rate

```
2
```

---

## Step 5: Configure Error Handling

If the workflow encounters an error:

```
Catch

↓

CloudWatch Logs

↓

Execution Failed
```

CloudWatch records all execution details to support troubleshooting and debugging.

---

## Step 6: Test the Workflow

Upload an image:

```
fire.jpg
```

to the bucket:

```
smart-campus-images
```

Check:

```
AWS Step Functions

↓

Executions

↓

Succeeded
```

If successful, the incident will be stored in Amazon DynamoDB and an alert email will be sent.

---

## Best Practices

✔ Design workflows using State Machines.

✔ Avoid direct Lambda-to-Lambda invocation.

✔ Configure a Retry Policy.

✔ Configure Error Handling.

✔ Send logs to Amazon CloudWatch.

✔ Separate each processing step for easier maintenance.

---

## Verification

After completing this chapter, you should have:

+ An AWS Step Functions State Machine.

+ A complete AI Workflow.

+ A Retry Policy.

+ Error Handling configuration.

+ CloudWatch Logs.

+ A workflow ready to integrate with Amazon Rekognition and Amazon Bedrock.

---

## Result

In this chapter, you successfully deployed AWS Step Functions to orchestrate the complete AI Workflow for the **Smart Campus Guardian** system.

AWS Step Functions connects the AI Lambda function, Amazon Rekognition, Amazon Bedrock, DynamoDB, SNS, and SES into an automated, reliable, and scalable workflow.

In the next chapter, you will deploy **Amazon Rekognition** to build the image object detection capability of the system.