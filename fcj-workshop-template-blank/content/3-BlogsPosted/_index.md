---
title: "Published Blog Posts"
date: 2026-07-21
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

This section contains three blog posts that I published on the **AWS Study Group** community, where I share my complete journey of building **Smart Notes API**—a serverless REST API for personal note management running entirely on Amazon Web Services (AWS).

### [Blog 1 - Building Smart Notes API](3.1-Blog1/)

Why I chose a **Serverless** architecture instead of Amazon EC2, how I designed the system using **Amazon API Gateway**, **AWS Lambda** (Express + `serverless-http`), **Amazon DynamoDB**, and **Amazon S3**, organized the code with **Clean Architecture**, and deployed the entire infrastructure as code using **AWS SAM**.

### [Blog 2 - From an Open API to Mandatory Authentication](3.2-Blog2/)

The journey of securing the API by first adding **API Keys** and **Usage Plans** in Amazon API Gateway to prevent anonymous access and abuse, then integrating **Amazon Cognito Hosted UI** with **Google Sign-In** to require user authentication before accessing notes.

### [Blog 3 - Real-World Operations: Automated CI/CD and Data Protection with PITR](3.3-Blog3/)

Automating deployments with **GitHub Actions CI/CD**, where unit tests always run before deployment, and protecting application data using **Amazon DynamoDB Point-in-Time Recovery (PITR)**, allowing the database to be restored to any point within the previous 35 days.