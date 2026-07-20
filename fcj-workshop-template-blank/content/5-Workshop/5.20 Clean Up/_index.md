---
title : "Cleaning Up Resources"
date : 2025-07-15
weight : 20
chapter : true
pre : " <b> 5.20. </b> "
---

# Cleaning Up Resources

## Introduction

After completing the **Smart Campus Guardian** Workshop, you should delete all AWS resources that were created to avoid unnecessary charges.

AWS recommends deleting resources in the correct order to prevent errors caused by dependencies between services.

---

# Objectives

After completing this chapter, you will be able to:

+ Remove the entire AWS infrastructure.
+ Ensure that no AWS services are still running.
+ Verify costs in AWS Billing.
+ Safely complete the workshop.

---

# Cleanup Architecture

The cleanup process is performed in the reverse order of the deployment architecture.

![Cleanup Architecture](/images/5-Workshop/5.20/5.20.1/cleanup-architecture.png)

---

# Cleanup Order

## Step 1

Delete the Amazon CloudFront Distribution.

```
CloudFront

↓

Disable Distribution

↓

Delete Distribution
```

Wait until the Distribution status changes to **Deleted** before proceeding.

---

## Step 2

Delete the website hosted on Amazon S3.

Bucket:

```
smart-campus-frontend
```

Delete all objects.

Then delete the bucket.

---

## Step 3

Delete the camera image bucket.

Bucket:

```
smart-campus-images
```

Delete all objects.

Then delete the bucket.

---

## Step 4

Delete the Amazon API Gateway.

```
SmartCampusGuardianAPI
```

↓

Delete API

---

## Step 5

Delete AWS Lambda Functions.

Delete the following Lambda functions:

```
DashboardFunction

IncidentFunction

CameraFunction

AIWorkflowFunction
```

---

## Step 6

Delete AWS Step Functions.

```
SmartCampusWorkflow
```

↓

Delete State Machine

---

## Step 7

Delete the Amazon EventBridge Rule.

```
SmartCampusImageUploaded
```

↓

Delete Rule

---

## Step 8

Delete the Amazon SNS Topic.

```
CampusAlertTopic
```

↓

Delete Topic

---

## Step 9

Delete Amazon SES Identity.

Navigate to:

```
Verified Identities
```

↓

Delete Identity

---

## Step 10

Delete the Amazon DynamoDB table.

Table:

```
Incident
```

↓

Delete Table

---

## Step 11

Delete the Amazon Cognito User Pool.

```
SmartCampusGuardianUserPool
```

↓

Delete User Pool

---

## Step 12

Delete IAM Roles.

```
SmartCampusLambdaRole

SmartCampusStepFunctionRole
```

↓

Delete Role

---

## Step 13

Check Amazon CloudWatch.

Delete:

+ Log Groups

+ Dashboard

+ Alarm

(if they were created during this workshop)

---

## Step 14

Check AWS Billing.

Navigate to:

```
AWS Billing

↓

Bills
```

Make sure that:

✔ No AWS services are still running.

✔ No additional charges are being incurred.

---

## Best Practices

✔ Delete resources in the recommended order.

✔ Delete all objects before deleting an Amazon S3 bucket.

✔ Check Amazon CloudWatch Log Groups.

✔ Review AWS Billing after completing the cleanup.

✔ Do not leave AWS resources running after the workshop.

---

## Summary

After completing this chapter, the entire AWS infrastructure for **Smart Campus Guardian** has been completely removed.

The workshop is now complete, and your AWS account will no longer incur charges for the deployed resources.

![Cleanup Result](/images/5-Workshop/5.20/5.20.2/cleanup-result.png)