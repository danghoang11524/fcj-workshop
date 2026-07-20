---
title : "Deploying the Dashboard and Frontend"
date : 2025-07-15
weight : 18
chapter : true
pre : " <b> 5.18. </b> "
---

# Deploying the Dashboard and Frontend

## Introduction

In this chapter, we will deploy the administration interface (Dashboard) for the **Smart Campus Guardian – AI Campus Incident Detection Platform**.

The frontend is developed using **ReactJS** and deployed as a **Static Website** on **Amazon S3**. The website is distributed through **Amazon CloudFront** to improve access speed and support HTTPS.

The Dashboard uses **Amazon Cognito** for user authentication, then calls **Amazon API Gateway** to retrieve data from AWS Lambda. Lambda queries **Amazon DynamoDB** to display incidents, camera status, and AI analysis results in real time.

---

# Objectives

After completing this chapter, you will be able to:

+ Deploy ReactJS to Amazon S3.
+ Distribute the website through Amazon CloudFront.
+ Sign in using Amazon Cognito.
+ Call Amazon API Gateway.
+ Display Incident data from DynamoDB.

---

# Dashboard Architecture

The Dashboard is the central management interface of the Smart Campus Guardian system.

Users access the website through Amazon CloudFront, sign in using Amazon Cognito, and retrieve data through Amazon API Gateway.

![Frontend Architecture](/images/5-Workshop/5.18/5.18.1/frontend-architecture.png)

---

## Architecture Overview

The workflow is as follows:

1. The administrator accesses the website through Amazon CloudFront.
2. CloudFront serves content from the Amazon S3 Static Website.
3. The user signs in using Amazon Cognito.
4. Cognito issues a JWT token.
5. The Dashboard sends requests to Amazon API Gateway.
6. API Gateway invokes AWS Lambda.
7. Lambda queries Amazon DynamoDB.
8. The Dashboard displays Incidents, Camera status, and AI Reports in real time.
9. Amazon CloudWatch records API logs and metrics.

---

# Dashboard Features

The Dashboard includes the following main features:

+ Login

+ Dashboard

+ Camera Management

+ Incident Management

+ AI Report

+ Alert History

---

# APIs Used

The Dashboard uses the following APIs:

```
GET /dashboard

GET /incident

GET /camera

GET /report
```

---

# Step 1: Build the ReactJS Application

Open a terminal:

```bash
npm install

npm run build
```

After the build is complete, the following directory will be generated:

```
build/
```

---

# Step 2: Upload the Website to Amazon S3

Navigate to:

```
Amazon S3

↓

smart-campus-frontend
```

Upload all contents of the:

```
build/
```

directory.

---

# Step 3: Update CloudFront

Navigate to:

```
Amazon CloudFront

↓

Distribution

↓

Invalidations

↓

Create Invalidation
```

Enter:

```
/*
```

Click:

```
Invalidate
```

Wait approximately 5–10 minutes for the distribution to update.

---

# Step 4: Sign In to the System

Access:

```
https://xxxxxxxx.cloudfront.net
```

Sign in using the Amazon Cognito account created in **Chapter 5.7**.

After successful authentication, Cognito issues a JWT token that the Dashboard uses to call Amazon API Gateway.

---

# Dashboard Interface

The Dashboard displays the following real-time information:

+ Today's Incidents

+ Cameras Online

+ High-Risk Alerts

+ AI Processing Status

+ Recent Alerts

+ System Health

![Dashboard](/images/5-Workshop/5.18/5.18.2/dashboard.png)

---

## Best Practices

✔ Build ReactJS in Production mode.

✔ Access the website only through Amazon CloudFront.

✔ Use JWT tokens issued by Amazon Cognito.

✔ Do not access DynamoDB directly from the frontend.

✔ Route all requests through Amazon API Gateway.

✔ Monitor logs using Amazon CloudWatch.

---

## Validation

After completing this chapter, you will have:

+ A website running on Amazon CloudFront.

+ User authentication with Amazon Cognito.

+ A Dashboard displaying Incident data.

+ A fully functional Amazon API Gateway.

+ Data retrieved from Amazon DynamoDB.

---

## Summary

In this chapter, you successfully deployed the Dashboard for the **Smart Campus Guardian** system.

The Dashboard enables administrators to monitor camera status, incidents, AI reports, and alert history in real time through a serverless architecture on AWS.

In the next chapter, we will test the complete AI Workflow and verify that all AWS services operate correctly according to the system design.