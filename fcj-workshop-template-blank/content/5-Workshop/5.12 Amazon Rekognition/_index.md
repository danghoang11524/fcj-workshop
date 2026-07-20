---
title : "Deploy Amazon Rekognition"
date : 2025-07-15
weight : 12
chapter : true
pre : " <b> 5.12. </b> "
---

# Deploy Amazon Rekognition

## Introduction

Amazon Rekognition is an AWS AI service that analyzes images and detects objects using Machine Learning technology.

In the **Smart Campus Guardian – AI Campus Incident Detection Platform** project, Amazon Rekognition is used to identify objects appearing in images uploaded by cameras to Amazon S3.

After AWS Step Functions triggers the AI Workflow, the AI Lambda function sends the image to Amazon Rekognition for analysis. The results are then passed to Amazon Bedrock to assess the risk level and generate response recommendations.

---

## Objectives

After completing this chapter, you will be able to:

+ Connect AWS Lambda to Amazon Rekognition.
+ Analyze images stored in Amazon S3.
+ Detect objects in images.
+ Return Labels and Confidence Scores.
+ Prepare data for Amazon Bedrock.

---

# Amazon Rekognition Architecture

Amazon Rekognition is the AI service responsible for image recognition in the Smart Campus Guardian AI Workflow.

The AI Lambda function sends images to Amazon Rekognition for analysis before forwarding the results to Amazon Bedrock.

![Rekognition Architecture](/images/5-Workshop/5.12/5.12.1/ekognition-architecture.png)

---

## Architecture Overview

The processing workflow operates as follows:

1. AWS Step Functions invokes the AI Lambda function.
2. The AI Lambda function retrieves an image from Amazon S3.
3. The AI Lambda function sends the image to Amazon Rekognition.
4. Amazon Rekognition detects objects and returns Labels with Confidence Scores.
5. The AI Lambda function receives the results and forwards them to Amazon Bedrock for contextual analysis.
6. Amazon CloudWatch records all logs and metrics.

Using Amazon Rekognition enables the system to automatically identify important objects before the AI evaluates the overall risk level.

---

# Detectable Objects

In this workshop, Amazon Rekognition detects the following objects:

+ Person

+ Fire

+ Smoke

+ Vehicle

+ Bicycle

+ Backpack

+ Helmet

+ Crowd

Additionally, Rekognition can be extended to detect many other object types depending on the system requirements.

---

# Deployment Steps

## Step 1: Open Amazon Rekognition

Sign in to the **AWS Management Console**.

Search for:

```
Amazon Rekognition
```

Choose:

```
Image Analysis
```

---

## Step 2: Prepare an Image

Use an image uploaded to the following bucket:

```
smart-campus-images
```

Example:

```
fire.jpg
```

---

## Step 3: Configure Detect Labels

In the AI Lambda function, use the following API:

```
DetectLabels
```

Confidence Threshold

```
90%
```

Max Labels

```
10
```

![Detect Labels](/images/5-Workshop/5.12/5.12.2/create-rekognition.png)

---

## Step 4: Analyze the Results

Example response:

```
Fire
Confidence: 98%

Smoke
Confidence: 96%

Person
Confidence: 99%
```

The AI Lambda function filters the required labels before sending them to Amazon Bedrock.

---

## Step 5: Send Data to Amazon Bedrock

After processing, the detected labels are sent to Amazon Bedrock to:

+ Assess the risk level.

+ Classify the incident.

+ Generate alert messages.

+ Recommend appropriate response actions.

---

## Step 6: Verify

Upload an image:

```
fire.jpg
```

Check:

```
CloudWatch

↓

Lambda Logs

↓

Amazon Rekognition Response
```

If successful, the Lambda function receives a list of Labels and Confidence Scores.

---

## Best Practices

To ensure high detection accuracy, follow these recommendations:

✔ Use only labels with a Confidence Score of 90% or higher.

✔ Filter out irrelevant labels.

✔ Do not store the complete Rekognition response in the database.

✔ Send only the required labels to Amazon Bedrock.

✔ Record logs using Amazon CloudWatch.

---

## Verification

After completing this chapter, you should have:

+ A working Amazon Rekognition service.

+ The AI Lambda function successfully connected to Rekognition.

+ A list of detected Labels.

+ Confidence Scores.

+ Data ready for Amazon Bedrock.

---

## Result

In this chapter, you successfully deployed Amazon Rekognition to detect objects in images for the **Smart Campus Guardian** system.

Amazon Rekognition detects important objects such as people, fire, smoke, and vehicles, providing input data for Amazon Bedrock to perform contextual analysis and assess the risk level.

In the next chapter, you will deploy **Amazon Bedrock** to build the AI-powered incident analysis and intelligent alerting capabilities.