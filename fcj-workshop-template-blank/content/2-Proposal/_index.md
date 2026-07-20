---
title: "Project Proposal"
date: 2025-07-15
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

{{% notice info %}}
📌 **This document presents the proposal for the Smart Campus Guardian – AI Campus Incident Detection Platform, deployed on Amazon Web Services (AWS).**
{{% /notice %}}

# Smart Campus Guardian

## AI Campus Incident Detection Platform

Smart Campus Guardian is an AI-powered campus safety monitoring system built on a Cloud Native architecture using Amazon Web Services (AWS). The system automatically detects incidents such as fire, smoke, abnormal crowd gatherings, and other safety-related events from camera images, then analyzes, stores, and delivers real-time alerts.

---

# 1. Executive Summary

Today, most educational institutions still rely on manual camera monitoring, resulting in delayed responses when incidents occur.

The Smart Campus Guardian project leverages AWS AI and Serverless services to automate the entire incident detection and response process.

The system follows an **Event-Driven Architecture**, enabling automatic processing whenever a new image is uploaded from a camera.

---

# 2. Problem Statement

## What problem does it solve?

Traditional surveillance systems have several limitations:

- Completely dependent on human operators.
- Difficult to detect incidents in real time.
- No automated risk assessment.
- No automatic alerting mechanism.
- Difficult to scale as the number of cameras increases.

---

## Solution

Smart Campus Guardian addresses these challenges by:

- Cameras uploading images to Amazon S3.
- Amazon EventBridge automatically detecting new events.
- AWS Step Functions orchestrating the AI workflow.
- Amazon Rekognition detecting objects in images.
- Amazon Bedrock analyzing risk levels and generating incident reports.
- AWS Lambda handling business logic.
- Amazon DynamoDB storing incident data.
- Amazon SNS and Amazon SES sending notifications.
- A real-time dashboard displaying monitoring results.

---

# Benefits and Return on Investment (ROI)

Deploying Smart Campus Guardian provides several advantages:

- Reduces incident detection time from several minutes to just a few seconds.
- Lowers operational staffing costs.
- Improves detection accuracy using AI.
- Serverless architecture minimizes infrastructure costs.
- Easily scales as additional cameras are deployed.

---

# 3. Solution Architecture

The solution is built using **Cloud Native**, **Serverless**, and **Event-Driven** architecture principles.

Workflow:

```
Camera
    │
    ▼
Amazon S3
    │
Object Created Event
    ▼
Amazon EventBridge
    │
    ▼
AWS Step Functions
    │
    ▼
AWS Lambda
    │
 ┌──┴──────────────┐
 ▼                 ▼
Amazon        Amazon
Rekognition   Bedrock
     │             │
     └──────┬──────┘
            ▼
     Amazon DynamoDB
            │
     ┌──────┴──────┐
     ▼             ▼
Amazon SNS    Amazon SES
            │
            ▼
      React Dashboard
```

![Architecture Diagram](/images/2-Proposal/smart-campus-architecture.png)

---

# AWS Services Used

- **Amazon S3**: Stores camera images.
- **Amazon EventBridge**: Orchestrates events when new images are uploaded.
- **AWS Step Functions**: Manages the AI workflow.
- **AWS Lambda**: Executes serverless business logic.
- **Amazon Rekognition**: Detects objects in images.
- **Amazon Bedrock**: Performs AI-powered risk analysis using Generative AI.
- **Amazon DynamoDB**: Stores incident records.
- **Amazon Cognito**: Handles user authentication.
- **Amazon API Gateway**: Provides REST APIs.
- **Amazon SNS**: Sends real-time notifications.
- **Amazon SES**: Sends email alerts.
- **Amazon CloudWatch**: Monitors system performance and logs.
- **Amazon CloudFront**: Delivers the web application globally.
- **IAM**: Manages identity and access permissions.

---

# Component Design

### Frontend

- ReactJS
- Amazon S3
- Amazon CloudFront
- Amazon Cognito

### Backend

- Amazon API Gateway
- AWS Lambda

### AI Layer

- Amazon Rekognition
- Amazon Bedrock

### Data Layer

- Amazon DynamoDB

### Monitoring

- Amazon CloudWatch

### Notification

- Amazon SNS
- Amazon SES

---

# 4. Technical Implementation

## Deployment Phases

### Phase 1

Prepare the AWS environment.

### Phase 2

Deploy IAM and Amazon S3.

### Phase 3

Deploy Amazon Cognito and Amazon API Gateway.

### Phase 4

Deploy AWS Lambda.

### Phase 5

Build the AI workflow using Amazon EventBridge and AWS Step Functions.

### Phase 6

Integrate Amazon Rekognition and Amazon Bedrock.

### Phase 7

Store incident data in Amazon DynamoDB.

### Phase 8

Deploy Amazon SNS, Amazon SES, and Amazon CloudWatch.

### Phase 9

Deploy the Dashboard.

### Phase 10

Perform end-to-end system testing.

---

## Technical Requirements

- AWS Account
- AWS IAM User
- AWS Region (ap-southeast-1)
- AWS CLI
- Visual Studio Code
- Node.js
- Python 3.12
- ReactJS
- Git

---

# 5. Timeline & Milestones

| Week | Task |
|------|------|
| Week 1 | Design the solution architecture |
| Week 2 | Deploy AWS infrastructure |
| Week 3 | Develop the backend |
| Week 4 | Build the AI workflow |
| Week 5 | Develop the dashboard and authentication |
| Week 6 | Test and finalize the system |

---

# 6. Cost Estimation

AWS Free Tier can be used for learning and testing purposes.

Reference:

https://calculator.aws

## Estimated Infrastructure Cost

| Service | Monthly Cost |
|----------|--------------|
| Amazon S3 | ~2 USD |
| AWS Lambda | Free Tier |
| Amazon DynamoDB | Free Tier |
| Amazon API Gateway | Free Tier |
| Amazon EventBridge | Free Tier |
| AWS Step Functions | Free Tier |
| Amazon CloudWatch | ~2 USD |
| Amazon SNS | Free Tier |
| Amazon SES | <1 USD |
| Amazon Rekognition | Based on the number of images processed |
| Amazon Bedrock | Based on token usage |

**Estimated total testing cost:** approximately **5–10 USD per month**.

---

# 7. Risk Assessment

## Risk Matrix

| Risk | Severity |
|------|----------|
| AI misclassification | Medium |
| Camera connection failure | High |
| Increasing AI costs | Medium |
| API overload | Low |
| Workflow failure | Low |

---

## Mitigation Strategy

- Configure CloudWatch Alarms.
- Enable Retry policies in Step Functions.
- Apply the IAM Least Privilege principle.
- Restrict API access using Amazon Cognito.
- Use Billing Alarms to monitor AWS costs.

---

## Contingency Plan

- Retry failed workflows.
- Store logs in Amazon CloudWatch.
- Back up critical data.
- Automatically send alerts when workflows fail.

---

# 8. Expected Outcomes

## Technical Improvements

- Build a complete Cloud Native system.
- Implement an Event-Driven architecture.
- Integrate AI Vision and Generative AI.
- Reduce incident processing time to just a few seconds.
- Fully automate the campus monitoring process.

---

## Long-Term Value

Smart Campus Guardian can be extended to support:

- Smart Campuses
- Smart Factories
- Hospitals
- Shopping Centers
- Office Buildings
- Industrial Parks
- Smart Cities

Smart Campus Guardian is a modern Cloud Native solution that leverages AWS AI Services and Serverless Computing to build an intelligent monitoring platform that is scalable, cost-efficient, and aligned with AWS architectural best practices.
````
