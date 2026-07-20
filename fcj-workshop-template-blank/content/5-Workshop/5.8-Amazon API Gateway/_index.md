---
title : "Deploy Amazon API Gateway"
date : 2025-07-14
weight : 8
chapter : true
pre : " <b> 5.8. </b> "
---

# Deploy Amazon API Gateway

## Introduction

Amazon API Gateway is an AWS service that enables you to build, manage, and secure REST APIs.

In the **Smart Campus Guardian – AI Campus Incident Detection Platform** project, Amazon API Gateway serves as the communication layer between the React web application and the backend services running on AWS Lambda.

In addition to request routing, API Gateway integrates directly with Amazon Cognito to validate JWT Tokens before allowing access to the system APIs.

---

## Objectives

After completing this chapter, you will be able to:

+ Create a REST API.
+ Create API Resources and Methods.
+ Connect API Gateway to AWS Lambda.
+ Configure JWT Authorization with Amazon Cognito.
+ Deploy the API to the Production stage.
+ Enable CloudWatch Logging.

---

# Amazon API Gateway Architecture

Amazon API Gateway acts as the API layer of the system, receiving all requests from the React website and forwarding them to AWS Lambda after validating the JWT Token.

![API Gateway Architecture](/images/5-Workshop/5.8/5.8.1/api-architecture.png)

---

## Architecture Overview

The request processing flow is as follows:

1. The user signs in to the React website.
2. Amazon Cognito issues a JWT Access Token.
3. The website includes the Access Token in the header of every API request.
4. Amazon API Gateway validates the JWT Token.
5. If the token is valid, API Gateway forwards the request to AWS Lambda.
6. AWS Lambda processes the request and returns the response to the website.
7. Amazon CloudWatch records API logs and metrics.

Using API Gateway enhances system security while providing centralized API management and seamless scalability.

---

# API Endpoints

| Method | Endpoint | Description |
|---------|----------|-------------|
| GET | /dashboard | Retrieve dashboard data |
| GET | /incidents | Retrieve the list of incidents |
| GET | /incidents/{id} | Retrieve incident details |
| POST | /camera/upload | Upload camera images |
| GET | /cameras | Retrieve the list of cameras |
| POST | /login | User authentication (Amazon Cognito) |

---

# Deployment Steps

## Step 1: Open Amazon API Gateway

Sign in to the **AWS Management Console**.

Search for:

```
Amazon API Gateway
```

Select:

```
Create API
```

Then choose:

```
REST API
```

---

## Step 2: Create a REST API

API Name

```
SmartCampusGuardianAPI
```

Endpoint Type

```
Regional
```

Description

```
REST API for Smart Campus Guardian
```

![Create API](/images/5-Workshop/5.8/5.8.2/create-api.png)

---

## Step 3: Create Resources

Create the following resources:

```
/dashboard

/incidents

/cameras

/camera/upload
```

These resources represent the main API groups of the system.

---

## Step 4: Create Methods

Create the required HTTP methods:

```
GET

POST
```

Then configure the integration:

```
Integration Type

↓

Lambda Function
```

Select the Lambda function:

```
SmartCampusHandler
```

---

## Step 5: Configure Authorization

Authorization Type

```
Amazon Cognito
```

User Pool

```
SmartCampusGuardianUserPool
```

Identity Source

```
Authorization Header
```

API Gateway will validate the JWT Token before forwarding requests to AWS Lambda.

---

## Step 6: Deploy the API

Select:

```
Deploy API
```

Stage Name

```
prod
```

After deployment, API Gateway provides an Invoke URL.

Example:

```
https://abc123.execute-api.ap-southeast-1.amazonaws.com/prod
```

---

## Step 7: Enable CloudWatch Logging

Within the Stage settings:

```
Logs / Tracing
```

Enable:

```
Enable CloudWatch Logs

Enable Access Logging
```

CloudWatch will record all API requests, responses, and errors.

---

## Step 8: Test the API

Use Postman or the React website to send a request:

```
GET /dashboard
```

Header

```
Authorization

Bearer <Access Token>
```

If the Access Token is valid, the API returns the dashboard data.

---

## Best Practices

To improve security and performance, follow these recommendations:

✔ Use JWT Authorization.

✔ Enable CloudWatch Logs.

✔ Enable Request Validation.

✔ Configure CORS.

✔ Configure Throttling.

✔ Use separate stages for Development and Production.

---

## Verification

After completing this chapter, you will have:

+ A working REST API.

+ A `prod` stage.

+ JWT Authorization using Amazon Cognito.

+ API Gateway integrated with AWS Lambda.

+ CloudWatch Logging enabled.

---

## Result

In this chapter, you successfully deployed Amazon API Gateway for the **Smart Campus Guardian** system.

API Gateway serves as the communication layer between the React website and AWS Lambda while handling JWT authentication, centralized API management, and logging through Amazon CloudWatch.

In the next chapter, you will deploy **AWS Lambda** to build the serverless backend that processes requests from Amazon API Gateway.