---
title : "Workshop"
date : 2025-07-15
weight : 5
chapter : true
pre : "<b>5.</b>"
---

# Workshop

## Introduction

In this section, we will deploy the **Smart Campus Guardian – AI Campus Incident Detection Platform** on **Amazon Web Services (AWS)**.

This system is a smart campus safety monitoring solution built using **Cloud-Native** and **Serverless** architectures, integrating AWS **AI/ML** services to automatically detect, analyze, and notify users of incidents such as fires, smoke, abnormal crowds, and other safety-related events.

The workshop follows an **Event-Driven Architecture**, where the entire workflow—from the moment a camera uploads an image to Amazon S3 until an alert is sent—is processed automatically through AWS services.

---

## Objectives

After completing this workshop, you will be able to:

- Deploy a complete Cloud-Native architecture on AWS.
- Build an event-processing system based on an Event-Driven Architecture.
- Integrate AI Vision with Amazon Rekognition for image analysis.
- Use Amazon Bedrock to assess risk levels and generate AI reports.
- Build REST APIs using Amazon API Gateway and AWS Lambda.
- Store data using Amazon DynamoDB.
- Send real-time notifications with Amazon SNS and Amazon SES.
- Monitor the system using Amazon CloudWatch.
- Deploy a React frontend on Amazon S3 and Amazon CloudFront.

---

## System Architecture

The system consists of the following main components:

- Amazon S3 for storing camera images.
- Amazon EventBridge for receiving events when new images are uploaded.
- AWS Step Functions for orchestrating the entire AI workflow.
- AWS Lambda for business logic processing.
- Amazon Rekognition for object detection in images.
- Amazon Bedrock for risk assessment and AI report generation.
- Amazon DynamoDB for storing incident information.
- Amazon SNS and Amazon SES for sending notifications.
- Amazon CloudWatch for monitoring and observability.
- Amazon Cognito for user authentication.
- Amazon API Gateway for providing application APIs.
- Amazon CloudFront for global website content delivery.

---

## Workshop Contents

The workshop consists of the following chapters:

**5.1.** Introduction

**5.2.** System Architecture

**5.3.** Environment Setup

**5.4.** Create IAM Users and IAM Roles

**5.5.** Deploy Amazon S3

**5.6.** AI Workflow

**5.7.** Deploy Amazon Cognito

**5.8.** Deploy Amazon API Gateway

**5.9.** Deploy AWS Lambda

**5.10.** Deploy Amazon EventBridge

**5.11.** Deploy AWS Step Functions

**5.12.** Deploy Amazon Rekognition

**5.13.** Deploy Amazon Bedrock

**5.14.** Deploy Amazon DynamoDB

**5.15.** Deploy Amazon SNS

**5.16.** Deploy Amazon SES

**5.17.** Monitor with Amazon CloudWatch

**5.18.** Dashboard and Frontend

**5.19.** System Testing

**5.20.** Resource Cleanup

**5.21.** Conclusion

---

## Expected Outcome

After completing this workshop, you will have successfully deployed a complete **AI Campus Incident Detection Platform** on AWS, including storage, AI processing, user management, APIs, databases, notification services, and monitoring.

The architecture follows the principles of the **AWS Well-Architected Framework**, ensuring security, scalability, high availability, and cost optimization.