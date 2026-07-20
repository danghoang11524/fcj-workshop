---
title : "Deploying Amazon SNS"
date : 2025-07-15
weight : 15
chapter : true
pre : " <b> 5.15. </b> "
---

# Deploying Amazon SNS

## Introduction

Amazon Simple Notification Service (Amazon SNS) is AWS's Publish/Subscribe (Pub/Sub) messaging service.

In the **Smart Campus Guardian – AI Campus Incident Detection Platform** project, Amazon SNS is used to distribute notifications whenever the system detects incidents with a **HIGH** or **CRITICAL** risk level.

After Amazon Bedrock completes the analysis and the AI Lambda function stores the incident in Amazon DynamoDB, the Lambda function publishes a message to an SNS Topic. All subscribers receive the notification in real time.

---

## Objectives

After completing this chapter, you will be able to:

+ Create an SNS Topic.
+ Create a Subscription.
+ Publish messages from the AI Lambda function.
+ Verify that notifications are delivered successfully.

---

# Amazon SNS Architecture

Amazon SNS is the real-time notification service in the AI workflow.

After an incident is stored in Amazon DynamoDB, the AI Lambda function publishes a message to an SNS Topic to notify subscribed systems or services.

![SNS Architecture](/images/5-Workshop/5.15/5.15.1/sns-architecture.png)

---

## Architecture Overview

The workflow operates as follows:

1. AWS Step Functions orchestrates the AI workflow.
2. The AI Lambda function receives the analysis results from Amazon Bedrock.
3. The AI Lambda function stores the incident in Amazon DynamoDB.
4. The AI Lambda function publishes a message to Amazon SNS.
5. Amazon SNS delivers the notification to all subscribers.
6. Amazon CloudWatch records logs and metrics.

Amazon SNS decouples AI processing from notification delivery, making the system easier to scale and integrate with additional services.

---

# Deployment Steps

## Step 1: Open Amazon SNS

Sign in to the **AWS Management Console**.

Search for:

```
Amazon SNS
```

Select:

```
Topics
```

↓

```
Create Topic
```

---

## Step 2: Create a Topic

Topic Type

```
Standard
```

Topic Name

```
SmartCampusAlertTopic
```

Display Name

```
Campus Alert
```

![Create SNS Topic](/images/5-Workshop/5.15/5.15.2/create-topic.png)

---

## Step 3: Create a Subscription

Select the newly created topic.

↓

```
Create Subscription
```

Protocol

```
Email
```

Endpoint

```
security@school.edu
```

> In a production environment, you can also use SMS, AWS Lambda, or HTTP endpoints as subscribers.

---

## Step 4: Update the AI Lambda Function

After the incident is stored in Amazon DynamoDB, the AI Lambda function uses the following API:

```
Publish
```

to send a notification to the topic:

```
SmartCampusAlertTopic
```

Example message:

```text
Incident Detected

Location: Building A

Risk Level: HIGH

Summary:
Fire detected near Building A.

Time:
2025-07-15 10:30 UTC
```

---

## Step 5: Verify

Upload an image:

```
fire.jpg
```

After the AI workflow is completed:

```
AI Lambda

↓

Amazon SNS

↓

Topic

↓

Messages Published
```

Verify that the **Messages Published** count is greater than 0.

---

## Best Practices

✔ Publish notifications only when the Risk Level is **HIGH** or **CRITICAL**.

✔ Do not include sensitive information in notification messages.

✔ Organize topics by notification category.

✔ Use IAM Roles instead of Access Keys.

✔ Record logs using Amazon CloudWatch.

---

## Verification

After completing this chapter, you will have:

+ An Amazon SNS Topic.

+ An active Subscription.

+ The AI Lambda function successfully publishing messages.

+ Real-time notifications.

---

## Summary

In this chapter, you successfully deployed Amazon SNS to send real-time notifications in the **Smart Campus Guardian** system.

Amazon SNS distributes notifications to subscribers immediately after the AI detects high-risk incidents while keeping the AI workflow decoupled from notification services.

In the next chapter, we will deploy **Amazon SES** to send official email alerts to administrators and the campus security team.