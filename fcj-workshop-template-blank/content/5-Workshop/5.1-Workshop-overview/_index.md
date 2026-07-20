```markdown id="intro51en"
---
title : "Introduction"
date : 2025-07-14
weight : 1
chapter : true
pre : " <b> 5.1. </b> "
---

# Introduction to Smart Campus Guardian

Smart Campus Guardian is an intelligent campus incident monitoring system built entirely on AWS Cloud using a **Cloud Native** and **Event-Driven** architecture.

The system leverages AWS AI services to automatically detect incidents such as:

+ Fire or smoke on campus.
+ A person falling or remaining motionless for an extended period.
+ Unusual crowd gatherings.
+ Unauthorized access to restricted areas.
+ Camera connection failures.

Once an incident is detected, the system automatically analyzes the level of risk, stores the incident information, sends alerts to the campus security team, and displays the results on the dashboard in real time.

---

#### Workshop Objectives

In this workshop, you will deploy an intelligent monitoring system using AWS services.

After completing the workshop, you will be able to:

+ Deploy a website using Amazon S3 and Amazon CloudFront.
+ Build a serverless API with Amazon API Gateway and AWS Lambda.
+ Store camera images in Amazon S3.
+ Build an event-processing workflow using Amazon EventBridge and AWS Step Functions.
+ Use Amazon Rekognition to detect objects in images.
+ Use Amazon Bedrock to assess incident risk levels.
+ Store incident data in Amazon DynamoDB.
+ Send email and SMS alerts using Amazon SES and Amazon SNS.
+ Monitor the entire system using Amazon CloudWatch.

---

#### System Architecture Overview

The Smart Campus Guardian architecture is divided into the following main components:

+ **Frontend Layer**  
The website is hosted on **Amazon S3 Static Website Hosting** and delivered through **Amazon CloudFront** to improve performance and reduce latency.

+ **Authentication Layer**  
Users authenticate through **Amazon Cognito**, using JWT tokens to securely access the APIs.

+ **Application Layer**  
User requests are received by **Amazon API Gateway** and processed by **AWS Lambda**.

+ **AI Processing Layer**  
When a camera uploads an image to Amazon S3, the event triggers **Amazon EventBridge** and **AWS Step Functions** to orchestrate the AI processing workflow.

+ **AI Analysis Layer**  
Amazon Rekognition detects objects in the image, and Amazon Bedrock evaluates the risk level and recommends appropriate response actions.

+ **Data Layer**  
Incident information is stored in **Amazon DynamoDB**, while original images and related files are stored in **Amazon S3**.

+ **Notification Layer**  
If a high-risk incident is detected, the system sends email notifications through **Amazon SES** and SMS notifications through **Amazon SNS**.

+ **Monitoring Layer**  
All logs, metrics, and alarms are managed by **Amazon CloudWatch** to support monitoring and system operations.

---

#### Deployment Architecture

In this workshop, you will deploy the following components:

+ **Frontend Layer**
  + Amazon S3
  + Amazon CloudFront
  + Amazon Cognito

+ **Application Layer**
  + Amazon API Gateway
  + AWS Lambda

+ **AI Workflow**
  + Amazon EventBridge
  + AWS Step Functions
  + Amazon Rekognition
  + Amazon Bedrock

+ **Data Storage**
  + Amazon DynamoDB
  + Amazon S3

+ **Notification**
  + Amazon SNS
  + Amazon SES

+ **Monitoring & Security**
  + Amazon CloudWatch
  + IAM Roles
  + AWS Secrets Manager

This workshop is designed to help learners become familiar with **Serverless Architecture**, **Event-Driven Architecture**, **AWS AI Services**, and the design principles of the **AWS Well-Architected Framework**.

---

![Smart Campus Architecture](/images/5-Workshop/5.1/smart-campus-architecture.png)
```
