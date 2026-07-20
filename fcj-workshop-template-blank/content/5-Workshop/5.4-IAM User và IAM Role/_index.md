---
title : "Create IAM User and IAM Role"
date : 2025-07-14
weight : 4
chapter : true
pre : " <b> 5.4. </b> "
---

# Create IAM User and IAM Role

## Introduction

In this chapter, we will configure **AWS Identity and Access Management (IAM)** to manage access permissions for users and AWS services in the **Smart Campus Guardian – AI Campus Incident Detection Platform**.

IAM enables authentication and authorization for AWS resources while ensuring the system follows the **Principle of Least Privilege**, granting only the permissions required by each component.

---

## Objectives

After completing this chapter, you will be able to:

- Create an IAM User to administer the workshop.
- Create an IAM Role for AWS Lambda.
- Create an IAM Role for AWS Step Functions.
- Understand how to use IAM Policies.
- Apply the Principle of Least Privilege.
- Avoid using Access Keys in source code.

---

# IAM Architecture

The system uses an **IAM User** to manage the AWS Management Console and **IAM Roles** to allow AWS services to securely access AWS resources.

![IAM Architecture](/images/5-Workshop/5.4/iam-architecture.png)

---

## Architecture Overview

In Smart Campus Guardian, access permissions are divided into two categories:

### IAM User

The IAM User is used by administrators to:

- Sign in to the AWS Management Console.
- Manage AWS resources.
- Deploy the workshop.

For this workshop, the IAM User will be granted the **AdministratorAccess** policy to simplify the deployment process.

### IAM Role

IAM Roles are used by AWS services instead of Access Keys.

The following services will assume IAM Roles:

- AWS Lambda
- AWS Step Functions

Through IAM Roles, these services can securely access:

- Amazon S3
- Amazon DynamoDB
- Amazon Rekognition
- Amazon Bedrock
- Amazon SNS
- Amazon CloudWatch

This approach improves security and follows the **Principle of Least Privilege**.

---

## Step 1: Open the IAM Console

Sign in to the **AWS Management Console**.

Search for:

```
IAM
```

Select:

```
Identity and Access Management (IAM)
```

---

## Step 2: Create an IAM User

In the IAM Console:

```
Users
        ↓
Create User
```

User name:

```
campus-admin
```

Enable:

```
Provide user access to the AWS Management Console
```

Select:

```
I want to create an IAM User
```

![Create User](/images/5-Workshop/5.4/5.4.1-IAM/create-user.png)

---

## Step 3: Attach Permissions to the IAM User

Select:

```
Attach policies directly
```

Attach the following policy:

```
AdministratorAccess
```

{{% notice warning %}}
This workshop uses the **AdministratorAccess** policy to simplify deployment.

In a production environment, you should create custom IAM policies and grant only the permissions required by each service according to the **Principle of Least Privilege**.
{{% /notice %}}

---

## Step 4: Create an IAM Role for AWS Lambda

In the IAM Console:

```
Roles
        ↓
Create Role
```

Trusted entity:

```
AWS Service
```

Service:

```
Lambda
```

Role name:

```
SmartCampusLambdaRole
```

![Create IAM Role](/images/5-Workshop/5.4/5.4.2/create-role.png)

---

## Step 5: Attach Policies to the Lambda Role

Attach the following policies:

- AmazonS3FullAccess
- AmazonDynamoDBFullAccess
- AmazonSNSFullAccess
- AmazonRekognitionFullAccess
- AmazonBedrockFullAccess
- CloudWatchLogsFullAccess

After attaching the required policies, choose **Create Role**.

---

## Step 6: Create an IAM Role for AWS Step Functions

Create another IAM Role.

Role name:

```
SmartCampusStepFunctionsRole
```

Trusted entity:

```
AWS Step Functions
```

Attach the following policies:

- AWSLambdaRole
- AmazonEventBridgeFullAccess

Complete the role creation.

---

## Best Practices

To improve the security of your AWS environment, follow these best practices:

- Do not hard-code Access Keys in source code.
- Use IAM Roles instead of Access Keys whenever possible.
- Follow the Principle of Least Privilege.
- Enable MFA for IAM Users.
- Regularly review and remove unused Access Keys.

---

## Result

After completing this chapter, you have:

- Successfully created the **campus-admin** IAM User.
- Created an IAM Role for AWS Lambda.
- Created an IAM Role for AWS Step Functions.
- Configured permissions according to the Principle of Least Privilege.
- Prepared the required permissions for deploying AWS services in the following chapters.

In the next chapter, we will create **Amazon S3 buckets** to store AI camera images and configure the events that trigger the system's processing workflow.