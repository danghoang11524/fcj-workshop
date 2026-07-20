---
title : "Environment Setup"
date : 2025-07-14
weight : 3
chapter : true
pre : " <b> 5.3. </b> "
---

# Environment Setup

## Introduction

Before deploying **Smart Campus Guardian – AI Campus Incident Detection Platform**, we need to prepare the development environment and AWS account.

This chapter describes the prerequisites required to ensure a smooth deployment process throughout the remaining workshop chapters.

---

# Deployment Environment

The following diagram illustrates the components that must be prepared before starting the workshop deployment.

![Workshop Prerequisites](/images/5-Workshop/5.3/prerequisite.png)

---

## Prerequisites

Before deployment, make sure you have prepared:

- AWS Account
- IAM Administrator User
- AWS CLI
- Visual Studio Code
- Git
- Node.js 20 or later
- Java 21
- A stable Internet connection

To keep the workshop consistent, we will use the following AWS Region:

```text
ap-southeast-1 (Singapore)
```

---

## Verify AWS CLI

After installing AWS CLI, verify the installation with:

```bash
aws --version
```

Example output:

```text
aws-cli/2.x.x Python/3.x Windows/64-bit
```

---

## Verify AWS Account Configuration

After configuring AWS CLI, run the following command to confirm the active AWS account:

```bash
aws sts get-caller-identity
```

Expected output:

```json
{
  "UserId": "...",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/WorkshopAdmin"
}
```

If the command succeeds, AWS CLI has been configured correctly.

---

## Required IAM Permissions

The account used for this workshop must have permission to manage the following AWS services:

- IAM
- Amazon S3
- Amazon CloudFront
- Amazon Cognito
- Amazon API Gateway
- AWS Lambda
- Amazon EventBridge
- AWS Step Functions
- Amazon Rekognition
- Amazon Bedrock
- Amazon DynamoDB
- Amazon SNS
- Amazon SES
- Amazon CloudWatch

{{% notice warning %}}
It is recommended to use an **IAM User** or **IAM Role** with Administrator permissions in a learning environment. Do not use the AWS Root account to deploy this workshop.
{{% /notice %}}

---

## Tools Used

The following tools will be used throughout the workshop:

| Tool | Purpose |
|----------|----------|
| AWS Management Console | Manage AWS resources |
| AWS CLI | Manage resources from the command line |
| Visual Studio Code | Develop source code |
| Git | Manage source code |
| Node.js | Run the React frontend |
| Java 21 | Develop backend services |

---

## Workshop Contents

After completing the environment preparation, we will deploy each system component according to the designed architecture.

The next chapters include:

1. IAM
2. Amazon S3
3. Amazon Cognito
4. Amazon API Gateway
5. AWS Lambda
6. Amazon EventBridge
7. AWS Step Functions
8. Amazon Rekognition
9. Amazon Bedrock
10. Amazon DynamoDB
11. Amazon SNS
12. Amazon SES
13. Amazon CloudWatch
14. Dashboard
15. Testing
16. Cleanup

---

## Result

After completing this chapter, you will have prepared the development environment, AWS account, and all required tools to begin deploying the **Smart Campus Guardian** system.

In the next chapter, we will start by configuring **IAM** and creating the access permissions required for the entire system.