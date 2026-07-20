---
title : "Incident Analysis with Amazon Bedrock"
date : 2025-07-15
weight : 13
chapter : true
pre : " <b> 5.13. </b> "
---

# Incident Analysis with Amazon Bedrock

## Introduction

Amazon Bedrock is AWS's Generative AI service that enables you to use Large Language Models (LLMs) without managing the underlying machine learning infrastructure.

In the **Smart Campus Guardian – AI Campus Incident Detection Platform** project, Amazon Bedrock receives analysis results from Amazon Rekognition to evaluate the severity of incidents, generate AI reports, and recommend appropriate response actions.

After AWS Step Functions orchestrates the workflow and the AI Lambda function receives labels from Amazon Rekognition, the Lambda function sends the data to Amazon Bedrock for contextual analysis before storing the results in Amazon DynamoDB.

---

## Objectives

After completing this chapter, you will be able to:

+ Connect the AI Lambda function to Amazon Bedrock.
+ Analyze data from Amazon Rekognition.
+ Evaluate incident severity.
+ Generate AI reports and response recommendations.
+ Normalize incident data before storing it in DynamoDB.

---

# Amazon Bedrock Architecture

Amazon Bedrock is the AI component responsible for contextual analysis and incident severity assessment in the Smart Campus Guardian system.

The AI Lambda function sends labels detected by Amazon Rekognition to Amazon Bedrock to generate an AI report before storing the results.

![Bedrock Architecture](/images/5-Workshop/5.13/5.13.1/bedrock-architecture.png)

---

## Architecture Overview

The workflow operates as follows:

1. AWS Step Functions invokes the AI Lambda function.
2. The AI Lambda function receives the analysis results from Amazon Rekognition.
3. The AI Lambda function creates a prompt and sends it to Amazon Bedrock.
4. Amazon Bedrock performs contextual analysis using a Generative AI model.
5. Bedrock returns:
   - Risk Level
   - Incident Summary
   - Recommended Action
6. The AI Lambda function normalizes the data and stores it in Amazon DynamoDB.
7. Amazon CloudWatch records logs and metrics throughout the process.

By using Amazon Bedrock, the system not only detects objects but also understands the context and provides appropriate response recommendations.

---

# Deployment Steps

## Step 1: Open Amazon Bedrock

Sign in to the **AWS Management Console**.

Search for:

```
Amazon Bedrock
```

Select:

```
Model Playground
```

---

## Step 2: Choose a Foundation Model

Recommended model:

```
Amazon Nova Lite
```

Or:

```
Claude 3 Haiku
```

Select a Foundation Model to perform the analysis.

![Model Playground](/images/5-Workshop/5.13/5.13.2/create-bedrock.png)

---

## Step 3: Create a Prompt

Example prompt:

```text
You are an AI assistant for a Smart Campus Security System.

Detected objects:

Fire: 98%
Smoke: 95%
Person: 2

Analyze the incident and return JSON with:

- Risk Level
- Summary
- Recommended Action
```

---

## Step 4: Review the Results

Example response:

```json
{
  "riskLevel": "HIGH",
  "summary": "Fire and smoke detected near Building A. Two people are present in the affected area.",
  "recommendedAction": "Notify campus security immediately and initiate evacuation procedures."
}
```

The AI Lambda function receives this response and normalizes the data before storing it in Amazon DynamoDB.

---

## Step 5: Store the Incident

After receiving the response from Amazon Bedrock:

```
AI Lambda

↓

Amazon DynamoDB
```

The following information is stored:

+ Incident ID
+ Timestamp
+ Risk Level
+ Summary
+ Recommended Action

---

## Step 6: Test

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

Amazon Bedrock Response
```

If successful, the AI Lambda function will receive the AI-generated report and store the incident in Amazon DynamoDB.

---

## Best Practices

✔ Send only the necessary labels to Amazon Bedrock.

✔ Do not send the entire image unless required.

✔ Standardize the JSON output.

✔ Use clear and maintainable prompts.

✔ Record logs using Amazon CloudWatch.

---

## Verification

After completing this chapter, you will have:

+ Amazon Bedrock up and running.

+ The AI Lambda function successfully connected to Bedrock.

+ AI-generated reports.

+ Risk level assessment.

+ Recommended actions.

+ Data ready to be stored in Amazon DynamoDB.

---

## Summary

In this chapter, you successfully deployed Amazon Bedrock to perform contextual analysis and assess incident severity in the **Smart Campus Guardian** system.

Amazon Bedrock transforms object detection results from Amazon Rekognition into meaningful AI-generated reports, enabling operators to make faster and more informed decisions.

In the next chapter, we will deploy **Amazon DynamoDB** to store all AI-generated incident information.