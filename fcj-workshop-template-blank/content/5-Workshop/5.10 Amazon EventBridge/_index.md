---
title : "Deploy Amazon EventBridge"
date : 2025-07-14
weight : 10
chapter : true
pre : " <b> 5.10. </b> "
---

# Deploy Amazon EventBridge

## Introduction

Amazon EventBridge is AWS's **Event Bus** service that enables communication between services using an **Event-Driven Architecture**.

In the **Smart Campus Guardian – AI Campus Incident Detection Platform** project, EventBridge is responsible for receiving events whenever a camera uploads an image to Amazon S3 and automatically triggering the AI Workflow through AWS Step Functions.

Using EventBridge reduces the dependency between services, improves scalability, and enables the system to process events from multiple cameras simultaneously.

---

## Objectives

After completing this chapter, you will be able to:

+ Create an EventBridge Rule.
+ Monitor **Object Created** events from Amazon S3.
+ Automatically trigger AWS Step Functions.
+ Verify that events are processed successfully.
+ Prepare the AI Workflow for the following chapters.

---

# Amazon EventBridge Architecture

Amazon EventBridge serves as the event orchestration hub of the Smart Campus Guardian system.

When a camera uploads an image to Amazon S3, EventBridge receives the event and triggers AWS Step Functions to start the AI analysis workflow.

![EventBridge Architecture](/images/5-Workshop/5.10/5.10.1/eventbridge-architecture.png)

---

## Architecture Overview

The workflow operates as follows:

1. A camera uploads an image to Amazon S3.
2. Amazon S3 generates an **Object Created** event.
3. Amazon EventBridge receives the event.
4. EventBridge evaluates the configured rule.
5. EventBridge triggers AWS Step Functions.
6. Step Functions starts the AI Workflow.

By using EventBridge, Amazon S3 does not need to invoke Lambda directly, making the architecture more flexible and easier to extend with additional processing workflows.

---

# Deployment Steps

## Step 1: Open Amazon EventBridge

Sign in to the **AWS Management Console**.

Search for:

```
Amazon EventBridge
```

Choose:

```
Rules
```

↓

```
Create Rule
```

---

## Step 2: Create a Rule

Rule Name

```
SmartCampusImageUploaded
```

Description

```
Trigger AI Workflow when image uploaded to Amazon S3
```

Event Bus

```
Default
```

Rule Type

```
Rule with an Event Pattern
```

![Create Rule](/images/5-Workshop/5.10/5.10.2/create-rule.png)

---

## Step 3: Configure the Event Pattern

Event Source

```
AWS Services
```

AWS Service

```
Amazon S3
```

Event Type

```
Object Created
```

Bucket Name

```
smart-campus-images
```

This rule monitors every new image uploaded to the bucket.

---

## Step 4: Configure the Target

Target

```
AWS Step Functions State Machine
```

State Machine

```
SmartCampusAIWorkflow
```

Whenever an event occurs, EventBridge automatically starts the state machine.

---

## Step 5: Create the Rule

Review the configuration.

Choose:

```
Create Rule
```

After creation, the rule status should be:

```
Enabled
```

---

## Step 6: Test the Event

Upload a sample image:

```
fire.jpg
```

to the bucket:

```
smart-campus-images
```

After a few seconds, check:

```
Amazon EventBridge

↓

Monitoring

↓

Invocations
```

If the rule is configured correctly, the **Invocations** count will increase.

At the same time, AWS Step Functions will be triggered.

---

## Best Practices

To improve scalability and maintainability, follow these recommendations:

✔ Do not let Amazon S3 invoke Lambda directly.

✔ Use EventBridge as the central Event Bus.

✔ Design the system using an Event-Driven Architecture.

✔ Allow a single event to trigger multiple workflows.

✔ Collect logs and metrics with Amazon CloudWatch.

---

## Verification

After completing this chapter, you should have:

+ An EventBridge Rule.

+ An Event Pattern monitoring Amazon S3.

+ A rule in the **Enabled** state.

+ AWS Step Functions triggered automatically.

+ Events ready for the AI Workflow.

---

## Result

In this chapter, you successfully deployed Amazon EventBridge to orchestrate events within the **Smart Campus Guardian** system.

Amazon EventBridge connects Amazon S3 with AWS Step Functions using an Event-Driven Architecture, providing the foundation for the automated AI analysis workflow.

In the next chapter, you will deploy **AWS Step Functions** to build the complete AI Workflow for the system.