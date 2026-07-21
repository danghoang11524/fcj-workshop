---
title: "Blog 1"
date: 2026-07-21
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Building Smart Notes API — My First Serverless REST API on AWS

During my recent internship, I built and deployed a personal project
called **Smart Notes API**—a Serverless REST API for note management
running entirely on Amazon Web Services (AWS). In this post, I'd like to
share the journey behind the project, from why I chose a Serverless
architecture to what I successfully implemented.

## Why Serverless?

Before starting the project, I had two options: launch an Amazon EC2
instance running Node.js 24/7 or build the application using a
Serverless architecture.

I chose Serverless for one simple reason: this is a personal project,
and traffic is close to zero most of the time. There is no reason to pay
for a server that sits idle all day. With Serverless, I only pay when
real requests are received.

## The Architecture

```text
Client (Static HTML)
        │
        ▼
Amazon API Gateway
        │
        ▼
AWS Lambda (Express + serverless-http)
        │
   ┌────┴────┐
   ▼         ▼
Amazon       Amazon S3
DynamoDB     (Attached Images)
(Notes Data)
```

The Lambda function runs an Express application through the
`serverless-http` library. This means I can continue writing familiar
Express code—routes, middleware, and controllers—while deploying it as a
Lambda function without managing any servers.

To keep the project maintainable as it grows, I organized the backend
using **Clean Architecture**:

```
Controller
      ↓
Service
      ↓
Repository
```

This separation makes the code easier to maintain and greatly simplifies
unit testing with **Jest**, allowing DynamoDB and Amazon S3 to be mocked
without calling real AWS services during tests.

## Everything as Code with AWS SAM

One aspect I'm particularly happy with is that I never manually created
AWS resources through the Console.

The entire infrastructure—including the DynamoDB table, S3 bucket,
Lambda function, API Gateway, and IAM Role—is defined inside a single
`template.yaml` file and deployed using:

```bash
sam build && sam deploy --guided
```

The biggest advantage of this approach is reproducibility. Even if the
entire stack is accidentally deleted, running the deployment command
again recreates the exact same infrastructure without remembering
individual Console configurations.

## A Minimal Frontend

Since this is a personal project, I deliberately avoided frontend
frameworks such as React or Vue.

The frontend consists of a single HTML, CSS, and JavaScript application
that communicates directly with the REST API using `fetch`.

The interface is inspired by a traditional **library card catalog**.
Each note appears as a vintage index card, and a "FILED" stamp is shown
after a successful save, giving the application a distinctive visual
style while keeping it lightweight.

## Final Result

After completing the project, I had a fully functional Serverless REST
API supporting complete CRUD operations, image upload and deletion, and
a simple web interface ready for immediate use.

This is only the beginning of the journey. In the next two blog posts, I
will cover the additional features I implemented afterward, including
security with API Keys and Amazon Cognito, as well as production-focused
topics such as CI/CD automation and data protection using
Point-in-Time Recovery (PITR).

**Read More:**

- **Blog 2:** Security with API Keys and Amazon Cognito  
  https://www.facebook.com/groups/awsstudygroupfcj/permalink/2220307962067497/?rdid=St8Z9r4R01fFMtL7#

- **Blog 3:** Automated CI/CD and Data Protection with PITR  
  https://www.facebook.com/groups/awsstudygroupfcj/permalink/2220307175400909/?rdid=4S6HwU2Sbma5XR0i#

- **Original Facebook Post (AWS Study Group FCJ)**  
  https://www.facebook.com/groups/awsstudygroupfcj/permalink/2220328232065470/?rdid=dfMOxtzDfBog3SYU#