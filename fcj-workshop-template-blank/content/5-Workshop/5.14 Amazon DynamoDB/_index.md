---
title : "Deploying Amazon DynamoDB"
date : 2025-07-15
weight : 14
chapter : true
pre : " <b> 5.14. </b> "
---

# Deploying Amazon DynamoDB

## Introduction

Amazon DynamoDB is AWS's fully managed NoSQL database service that provides automatic scaling, high performance, and low-latency access.

In the **Smart Campus Guardian – AI Campus Incident Detection Platform** project, Amazon DynamoDB is used to store all incident information after Amazon Bedrock completes the AI analysis.

The AI Lambda function receives the analysis results from Amazon Bedrock, normalizes the data, and writes it to a DynamoDB table so that the Dashboard can query and display the information in real time.

---

## Objectives

After completing this chapter, you will be able to:

+ Create the Incident table.
+ Configure On-Demand capacity mode.
+ Store AI analysis results from the AI Lambda function.
+ Query data for the Dashboard.

---

# Amazon DynamoDB Architecture

Amazon DynamoDB stores all incident data after the AI analysis is completed.

The AI Lambda function writes data to DynamoDB, and the Dashboard reads the data for administrators to monitor incidents.

![DynamoDB Architecture](/images/5-Workshop/5.14/5.14.1/dynamodb.png)

---

## Architecture Overview

The workflow operates as follows:

1. AWS Step Functions orchestrates the AI workflow.
2. The AI Lambda function receives the analysis results from Amazon Bedrock.
3. The AI Lambda function normalizes the incident data.
4. The AI Lambda function uses the **PutItem** API to write data to Amazon DynamoDB.
5. The Dashboard, through API Gateway and Lambda, uses the **Scan** or **Query** API to retrieve the data.
6. Amazon CloudWatch records logs and metrics throughout the process.

Using DynamoDB enables the system to scale automatically and process thousands of events per day without managing database servers.

---

# Table Design

Table Name

```
Incident
```

Partition Key

```
IncidentId (String)
```

Stored attributes:

```
CameraId
Location
RiskLevel
Status
CreatedAt
Summary
ImageUrl
RecommendedAction
```

---

# Deployment Steps

## Step 1: Open Amazon DynamoDB

Sign in to the **AWS Management Console**.

Search for:

```
Amazon DynamoDB
```

Select:

```
Create Table
```

---

## Step 2: Create the Table

Table Name

```
Incident
```

Partition Key

```
IncidentId
```

Type

```
String
```

![Create Table](/images/5-Workshop/5.14/5.14.2/create-table.png)

---

## Step 3: Choose the Billing Mode

Billing Mode

```
On-Demand
```

This mode is ideal for the workshop because it does not require traffic forecasting and charges only for the actual number of requests.

---

## Step 4: Create the Table

Click:

```
Create Table
```

Wait until the table status changes to:

```
Active
```

---

## Step 5: Store Data from the AI Lambda Function

After Amazon Bedrock returns the analysis results, the AI Lambda function uses the following API:

```
PutItem
```

to write data to the:

```
Incident
```

table.

Example data:

```json
{
  "IncidentId": "INC-0001",
  "CameraId": "CAM-01",
  "RiskLevel": "HIGH",
  "Status": "OPEN",
  "Summary": "Fire detected near Building A.",
  "RecommendedAction": "Notify campus security immediately.",
  "CreatedAt": "2025-07-15T10:30:00Z",
  "ImageUrl": "s3://smart-campus-images/fire.jpg"
}
```

---

## Step 6: Verify

Upload an image:

```
fire.jpg
```

After the AI workflow is completed:

```
AWS Lambda

↓

Amazon DynamoDB

↓

Items

↓

Incident
```

Verify that the data has been stored successfully.

---

## Best Practices

✔ Use **On-Demand** capacity mode to optimize costs.

✔ Store only the image URL instead of the image file itself.

✔ Design a short and unique partition key.

✔ Normalize data before writing it to DynamoDB.

✔ Record logs using Amazon CloudWatch.

---

## Verification

After completing this chapter, you will have:

+ An Incident table in DynamoDB.

+ The AI Lambda function successfully writing data.

+ A Dashboard capable of querying incident data.

+ Incident records stored successfully.

---

## Summary

In this chapter, you successfully deployed Amazon DynamoDB to store all incident information for the **Smart Campus Guardian** system.

Amazon DynamoDB serves as the central database, enabling the Dashboard to quickly retrieve incident information while supporting other AWS services in monitoring the system's operational status.

In the next chapter, we will deploy **Amazon SNS** to send real-time alerts whenever high-risk incidents are detected.