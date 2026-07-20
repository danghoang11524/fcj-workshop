---
title : "Deploying Amazon SES"
date : 2025-07-15
weight : 16
chapter : true
pre : " <b> 5.16. </b> "
---

# Deploying Amazon SES

## Introduction

Amazon Simple Email Service (Amazon SES) is AWS's highly scalable and reliable email delivery service.

In the **Smart Campus Guardian – AI Campus Incident Detection Platform** project, Amazon SES is used to send detailed alert emails after the AI completes its incident analysis.

After AWS Step Functions orchestrates the AI workflow, the AI Lambda function stores the incident in Amazon DynamoDB, publishes a notification through Amazon SNS, and simultaneously uses Amazon SES to send an email to the management and security teams.

---

## Objectives

After completing this chapter, you will be able to:

+ Verify an email address with Amazon SES.
+ Send emails using AWS Lambda.
+ Configure an email template.
+ Verify that alert emails are sent successfully.

---

# Amazon SES Architecture

Amazon SES is the email delivery service used in the AI workflow.

After the AI Lambda function finishes processing an incident, the system uses Amazon SES to send an alert email to administrators.

![SES Architecture](/images/5-Workshop/5.16/5.16.1/ses-architecture.png)

---

## Architecture Overview

The workflow operates as follows:

1. AWS Step Functions orchestrates the AI workflow.
2. The AI Lambda function receives the analysis results from Amazon Bedrock.
3. The AI Lambda function stores the incident in Amazon DynamoDB.
4. The AI Lambda function publishes a notification to Amazon SNS.
5. The AI Lambda function calls Amazon SES to send an email.
6. Amazon CloudWatch records logs and metrics.

Using Amazon SES as a separate service makes it easier to modify email templates or support additional notification types without affecting the AI workflow.

---

# Deployment Steps

## Step 1: Open Amazon SES

Sign in to the **AWS Management Console**.

Search for:

```
Amazon SES
```

Select:

```
Verified Identities
```

↓

```
Create Identity
```

---

## Step 2: Verify an Email Address

Identity Type

```
Email Address
```

Email

```
security@school.edu
```

Click:

```
Create Identity
```

Then check your inbox and confirm the verification email.

![Verify Email](/images/5-Workshop/5.16/5.16.2/verify-email.png)

---

## Step 3: Update the AI Lambda Function

After the incident has been successfully stored, the AI Lambda function uses the following API:

```
SendEmail
```

to send an email to the verified address.

---

## Step 4: Email Content

Subject

```
Smart Campus Guardian - Incident Alert
```

Body

```text
Incident Detected

Location:
Building A

Risk Level:
HIGH

Summary:
Fire and smoke detected near Building A.

Recommended Action:
Notify campus security immediately and initiate evacuation.

Time:
2025-07-15 10:30 UTC
```

In a production environment, it is recommended to use email templates for easier management and reuse.

---

## Step 5: Verify

Upload an image:

```
fire.jpg
```

After the AI workflow is completed:

```
AI Lambda

↓

Amazon SES

↓

Sent Emails
```

Verify that the email has been sent to:

```
security@school.edu
```

---

## Best Practices

✔ Verify email addresses before sending emails.

✔ Use email templates.

✔ Do not hard-code email addresses in the source code.

✔ Send emails only when the Risk Level is **HIGH** or **CRITICAL**.

✔ Record logs using Amazon CloudWatch.

---

## Verification

After completing this chapter, you will have:

+ A verified Amazon SES identity.

+ The AI Lambda function successfully sending emails.

+ Automated incident alert emails.

+ CloudWatch logs recording the email delivery process.

---

## Summary

In this chapter, you successfully deployed Amazon SES to send alert emails in the **Smart Campus Guardian** system.

Amazon SES delivers detailed incident information—including the risk level, location, timestamp, and recommended actions—to the management team, helping improve response times during emergency situations.

In the next chapter, we will deploy **Amazon CloudWatch** to monitor the entire system, including logs, metrics, and alarms for the AI workflow.