---
title : "System Architecture"
date : 2025-07-14
weight : 2
chapter : true
pre : " <b> 5.2. </b> "
---

# System Architecture

## Introduction

In this chapter, we will explore the overall architecture of the **Smart Campus Guardian – AI Campus Incident Detection Platform**.

The system is built using a **Cloud Native** approach, combining **Serverless Architecture** and **Event-Driven Architecture** on **Amazon Web Services (AWS)**. This architecture enables the system to scale automatically, reduce operational costs, and process AI camera events in real time.

The entire workflow—from uploading camera images to Amazon S3 to sending alerts to administrators—is handled automatically using AWS Managed Services.

---

# Overall Architecture

The system architecture is divided into the following main layers:

- Frontend Layer
- Authentication Layer
- API Layer
- Event Processing Layer
- AI Analysis Layer
- Data Layer
- Notification Layer
- Monitoring Layer

Each layer has a dedicated responsibility, ensuring the system remains scalable, maintainable, and aligned with the AWS Well-Architected Framework.

---

## Deployment Architecture

The figure below illustrates the complete architecture of the Smart Campus Guardian system on AWS.

![System Architecture](/images/5-Workshop/5.2/5.2.1/system-architecture.png)

### Architecture Overview

#### Frontend Layer

The user interface is developed using **ReactJS** and hosted on **Amazon S3 Static Website Hosting**.

Users access the application through **Amazon CloudFront**, which provides:

- Global content delivery
- HTTPS support
- Reduced latency
- Efficient content distribution

---

#### Authentication Layer

The system uses **Amazon Cognito** for user management and authentication.

Amazon Cognito is responsible for:

- User registration
- User sign-in
- User Pool management
- Issuing JWT Access Tokens

After successful authentication, users use the Access Token to access APIs through Amazon API Gateway.

---

#### API Layer

Requests from the dashboard are sent to **Amazon API Gateway**.

API Gateway is responsible for:

- User authentication
- Request routing
- Invoking AWS Lambda backend functions
- Logging API activities

The Lambda backend executes business logic and retrieves data from Amazon DynamoDB.

---

#### Event Processing Layer

When a camera uploads an image to the **Amazon S3 Image Bucket**, the event processing pipeline is triggered automatically.

Amazon EventBridge detects the **Object Created** event and starts **AWS Step Functions** to orchestrate the AI workflow.

Thanks to the Event-Driven architecture, the system can process events from multiple cameras simultaneously without manual intervention.

---

#### AI Analysis Layer

This is the core component of the system.

Once the Step Functions workflow is triggered:

- AWS Lambda preprocesses the image.
- Amazon Rekognition detects objects in the image.
- Amazon Bedrock analyzes the detection results using Generative AI.

The system can identify situations such as:

- Fire
- Smoke
- Crowd
- Suspicious Person
- Vehicle

Amazon Bedrock then evaluates the risk level, generates an AI incident report, and recommends appropriate actions.

---

#### Data Layer

The system uses **Amazon DynamoDB** to store application data, including:

- Incident
- Camera Information
- AI Report
- Alert History

Meanwhile, Amazon S3 stores:

- Original images
- Reports
- AI output data

---

#### Notification Layer

When the AI determines that an incident has a **HIGH** or **CRITICAL** risk level, the system automatically sends alerts.

Amazon SNS is used to deliver real-time notifications.

Amazon SES is used to send email alerts to administrators.

---

#### Monitoring Layer

The entire system is monitored using **Amazon CloudWatch**.

CloudWatch collects:

- Logs
- Metrics
- Dashboards
- Alarms

This enables administrators to monitor system performance and quickly identify and troubleshoot issues.

---

# AI Workflow

In addition to the overall architecture, the system includes an automated AI workflow for processing images captured by cameras.

The workflow is orchestrated by Amazon EventBridge and AWS Step Functions, while Amazon Rekognition and Amazon Bedrock work together to analyze image content and make intelligent decisions.

![AI Workflow](/images/5-Workshop/5.2/5.2.2/ai-workflow.png)

---

## AI Processing Flow

The AI processing workflow consists of the following steps:

1. A camera uploads an image to Amazon S3.
2. Amazon S3 generates an **Object Created** event.
3. Amazon EventBridge detects the event.
4. AWS Step Functions starts the AI processing workflow.
5. AWS Lambda preprocesses the image.
6. Amazon Rekognition detects objects in the image.
7. Amazon Bedrock evaluates the risk level and generates an AI incident report.
8. AWS Lambda stores the results in Amazon DynamoDB.
9. Amazon SNS sends real-time notifications.
10. Amazon SES sends email alerts to administrators.
11. The Dashboard retrieves and displays incident information through Amazon API Gateway.
12. Amazon CloudWatch records logs and metrics for the entire system.

---

## Result

After completing this chapter, you will have a comprehensive understanding of the Smart Campus Guardian architecture and how AWS services work together throughout the system.

In the following chapters, we will deploy each architectural component step by step, beginning with environment preparation, IAM configuration, Amazon S3, and the AI services on AWS.