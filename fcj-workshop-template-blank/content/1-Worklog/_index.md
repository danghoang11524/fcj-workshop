---
title: "Worklog"
date: 2026-07-25
weight: 1
chapter: true
pre: " <b> 1. </b> "
---

This worklog covers a **12-week period (from May 4, 2026, to July 27, 2026)** throughout the internship. The primary objective was to design and develop the **Smart Notes API** backend—a fully **serverless REST API** on AWS that provides secure note management with user authentication, multi-layer security, data backup, and automated deployment. The weekly progress is summarized below:

**Week 1:** [Orientation & AWS Basics](1.1-week1/) — Became familiar with AWS, created an AWS account and IAM User, explored the core AWS services used in the project (Lambda, API Gateway, DynamoDB, S3, and Cognito), installed AWS CLI and AWS SAM CLI, and planned the overall system architecture.

**Week 2:** [IAM, S3 & CLI](1.2-week2/) — Configured IAM Roles and Policies following the Principle of Least Privilege, created an Amazon S3 bucket for storing note attachments, practiced AWS CLI operations, and learned AWS SAM for Infrastructure as Code (IaC).

**Week 3:** [Lambda & API Gateway](1.3-week3/) — Developed the first AWS Lambda function using Node.js 22 with Express and serverless-http, integrated it with Amazon API Gateway, configured basic REST routes, and tested the API using Postman.

**Week 4:** [CRUD API - Notes](1.4-week4/) — Implemented complete CRUD endpoints (Create, Read, Update, Delete) for notes following the Clean Architecture pattern (Controller → Service → Repository). Integrated image upload and deletion with Amazon S3, including a retry mechanism for failed uploads.

**Week 5:** [Authentication - Cognito](1.5-week5/) — Integrated Amazon Cognito User Pool and Hosted UI, configured email/password authentication, enabled Google Sign-In through Google Identity Provider, and applied a Cognito Authorizer to secure all API endpoints.

**Week 6:** [DynamoDB Integration](1.6-week6/) — Designed the Amazon DynamoDB **Notes** table using `userId` and `noteId` as the primary key to isolate user data, and migrated the Repository layer to use DynamoDB in **PAY_PER_REQUEST** mode.

**Week 7:** [Advanced API Features](1.7-week7/) — Added advanced backend features including pagination, server-side search, note tagging, and note pinning. Improved API response formatting and input validation.

**Week 8:** [Deployment & CI/CD](1.8-week8/) — Defined the entire serverless infrastructure using AWS SAM (`template.yaml`) and configured a CI/CD pipeline to automatically execute `sam build` and `sam deploy` whenever code is pushed to the Git repository, minimizing manual deployment errors.

**Week 9:** [Logging & Monitoring](1.9-week9/) — Configured Amazon CloudWatch Logs for each Lambda function, established a workflow for troubleshooting, and monitored system performance in real time.

**Week 10:** [Optimization & Security](1.10-week10/) — Enhanced system security by configuring API Keys and Usage Plans to enforce request throttling and quotas, enabled Point-in-Time Recovery (PITR) for DynamoDB to protect data, reviewed IAM permissions, and configured AWS Billing Alarms.

**Week 11:** [Testing & Documentation](1.11-week11/) — Performed comprehensive system testing, including functional testing, security testing, DynamoDB PITR recovery validation, and load testing with Artillery/k6. Completed the technical documentation and user guide.

**Week 12:** [Final Demo & Report](1.12-week12/) — Finalized the static web interface connected to the API, summarized the entire development process, prepared the final system demonstration, and completed the internship report.
```
