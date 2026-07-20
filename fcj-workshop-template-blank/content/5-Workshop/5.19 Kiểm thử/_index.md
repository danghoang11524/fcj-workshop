---
title : "System Testing"
date : 2025-07-15
weight : 19
chapter : true
pre : " <b> 5.19. </b> "
---

# System Testing

## Introduction

After completing the deployment of the entire AWS infrastructure, we will perform end-to-end testing of the **Smart Campus Guardian – AI Campus Incident Detection Platform**.

The objective of this chapter is to verify that the entire AI Workflow functions correctly, from the moment a user accesses the Dashboard to the point where the system detects an incident, performs AI analysis, and sends notifications.

---

# Objectives

After completing this chapter, you will be able to:

+ Test the Dashboard.
+ Test Authentication.
+ Test Amazon API Gateway.
+ Test the AI Workflow.
+ Test Notifications.
+ Test Monitoring.

---

# Testing Architecture

The testing process follows the system architecture.

![Testing Architecture](/images/5-Workshop/5.19/5.19.1/testing-architecture.png)

---

## Workflow Overview

The testing workflow consists of the following steps:

1. The administrator signs in to the Dashboard.
2. The Dashboard calls Amazon API Gateway.
3. AWS Lambda processes the request.
4. The camera uploads an image to Amazon S3.
5. Amazon EventBridge receives the event.
6. AWS Step Functions starts the AI Workflow.
7. Lambda AI calls Amazon Rekognition.
8. Amazon Bedrock evaluates the risk level.
9. Lambda AI stores the Incident in Amazon DynamoDB.
10. Lambda AI sends notifications through Amazon SNS and Amazon SES.
11. Amazon CloudWatch records logs and metrics.

---

# Test Case 1

## Dashboard Login

Access:

```
https://xxxxxxxx.cloudfront.net
```

Sign in using your Amazon Cognito account.

Expected Results

✔ Successful login

✔ JWT token issued

✔ Access to the Dashboard

---

# Test Case 2

## Test Amazon API Gateway

Open the Dashboard.

The Dashboard sends requests to:

```
GET /dashboard

GET /incident

GET /camera
```

Expected Results

✔ HTTP 200 response

✔ Data returned successfully

---

# Test Case 3

## Upload an Image

Upload:

```
fire.jpg
```

to the bucket:

```
smart-campus-images
```

Expected Results

✔ Image uploaded successfully

✔ Amazon EventBridge receives the event

---

# Test Case 4

## Test the AI Workflow

Open:

```
AWS Step Functions
```

↓

```
Executions
```

Expected Result

```
Succeeded
```

---

# Test Case 5

## Amazon Rekognition

Check the Amazon CloudWatch logs.

Expected Results

```
Fire

Smoke

Person
```

are detected.

---

# Test Case 6

## Amazon Bedrock

Review the AI analysis results.

Example:

```
Risk Level

HIGH

Summary

Fire detected near Building A

Recommended Action

Notify campus security immediately.
```

---

# Test Case 7

## Amazon DynamoDB

Open:

```
Amazon DynamoDB

↓

Incident
```

Expected Result

A new Incident record is created.

---

# Test Case 8

## Amazon SNS

Open:

```
Amazon SNS

↓

Topic

↓

Metrics
```

Check:

```
Messages Published
```

Expected Result

Greater than 0.

---

# Test Case 9

## Amazon SES

Check the email inbox:

```
security@school.edu
```

Expected Result

A notification email is received.

---

# Test Case 10

## Dashboard

The Dashboard displays:

+ Today's Incidents

+ High-Risk Alerts

+ AI Report

+ Camera Status

The displayed data must match the data stored in Amazon DynamoDB.

---

# Test Case 11

## Amazon CloudWatch

Check:

+ Lambda Logs

+ API Gateway Logs

+ Step Functions Logs

+ Metrics

+ Dashboard

Expected Result

No errors are detected.

---

# Summary

After completing this chapter, the entire **Smart Campus Guardian** system should be operating correctly according to the designed architecture.

The Dashboard displays real-time data, the AI Workflow executes successfully, notifications are delivered, and Amazon CloudWatch records all logs and metrics.

![Testing Result](/images/5-Workshop/5.19/5.19.2/testing-result.png)