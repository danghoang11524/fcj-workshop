---
title: "Proposal"
date: 2026-07-25
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Smart Notes API

## A Serverless REST API Platform for Notes Management

**Smart Notes API** is a REST API system for managing notes, built on a **fully Serverless** architecture on Amazon Web Services (AWS). The system allows users to create, view, edit, and delete notes, and attach images; it comes with a static web interface styled after a "library card catalog"; it includes login authentication (email/password or Google) and multi-layer API security at the gateway level.

| | |
|---|---|
| **Project status** | Completed |
---

# 1. Executive Summary

Simple personal note-taking tools typically force users/development teams to choose between two trade-offs:

1. **Using an off-the-shelf SaaS service** — fast, but loses control over data, creates vendor dependency, and makes business customization difficult.
2. **Self-hosting a traditional server (VPS/on-premise)** — more control, but incurs 24/7 maintenance costs even when actual usage is very low, and adds operational burden (OS patching, scaling, load balancing, manual backups).

The **Smart Notes API** project solves this problem with a **fully Serverless** architecture on AWS:

- No server runs continuously → **cost is only incurred with actual requests**.
- **Auto-scaling** based on traffic volume with no additional configuration needed.
- **Full control over data** (DynamoDB + S3, private within the deployer's own AWS account), with no dependency on a third-party SaaS provider.
- Applies **Clean Architecture** (Controller → Service → Repository) directly at the Lambda layer, making the code easy to unit test, maintain, and extend with new business logic over time.

Beyond basic note CRUD functionality, the system also integrates elements that are often overlooked in similar projects but are **essential in a real production system**: user authentication (Cognito), API abuse protection (Usage Plan/API Key), data backup (PITR), error monitoring (CloudWatch), and deployment automation (CI/CD).

---

# 2. Problem Statement

## 2.1. What is the problem?

When building a notes-management API the traditional way (a server running continuously), the system typically faces the following limitations:

| Limitation | Consequence |
|---|---|
| Server runs 24/7 even though there are no requests most of the time | Wasted infrastructure cost, especially for small-scale/personal applications |
| Difficult to auto-scale when the number of users spikes | System bottlenecks, poor user experience, requires manual intervention |
| Managing infrastructure (OS patching, scaling, load balancing) takes significant effort | Time spent on operations instead of feature development |
| A publicly exposed API with no protective layer | Easily scanned by bots, abused, or subjected to brute-force or application-layer DDoS attacks |
| No user authentication mechanism | Anyone who knows the URL can call the API, risking data leaks or tampering |
| No automatic data backup | Risk of permanent data loss from accidental delete/edit operations |
| Manual deployment | Error-prone, hard to reproduce environments, hard to roll back when issues occur |

## 2.2. Proposed Solution

Smart Notes API addresses these problems with a cohesive serverless architecture, in which each AWS component handles exactly one role:

- **Amazon API Gateway** receives all REST requests (GET/POST/PUT/DELETE), serving as the system's single entry point.
- **AWS Lambda** (Node.js 22, Express + serverless-http) handles business logic following the Clean Architecture model, running only when there is a request and terminating automatically afterward.
- **Amazon DynamoDB** stores note data in **PAY_PER_REQUEST** mode, with no need to provision capacity in advance.
- **Amazon S3** stores images attached to notes, with a retry mechanism for failed uploads.
- **Amazon API Gateway Usage Plan + API Key** throttles and limits API call rates to prevent abuse.
- **Amazon Cognito** (Hosted UI + Google linking) requires login before accessing notes, ensuring each user's data is isolated.
- **Static web interface** (plain HTML/CSS/JS) calls the API directly via fetch/CORS, requiring no frontend framework, reducing complexity and maintenance cost.

## 2.3. Benefits and Return on Investment (ROI)

| Criterion | Traditional Approach | Smart Notes API (Serverless) |
|---|---|---|
| Cost when there is no traffic | Still incurs 24/7 server costs | Nearly zero (Free Tier covers most services) |
| Infrastructure operations | Requires manual patching, upgrades, and server monitoring | No server management required |
| Scalability | Requires manual auto-scaling/load-balancer configuration | Automatically scales with request volume |
| Security | Must be built from scratch | Built-in IAM Least Privilege: each Lambda has exactly the permissions it needs on DynamoDB/S3 |
| Reusability | Tightly coupled to specific infrastructure | Architecture can be reused for other CRUD APIs (to-do list, bookmarks, journal, etc.) |

---

# 3. Solution Architecture

The architecture follows a **Serverless** model combined with **Clean Architecture** at the application layer.

## 3.1. Workflow Diagram

```
 Client (Static web: HTML/CSS/JS)
          │
          │  fetch/CORS + x-api-key + Authorization (Cognito ID Token)
          ▼
   Amazon API Gateway (REST API)
          │
          │  Cognito Authorizer (login required)
          │  + Usage Plan / API Key (throttle & quota)
          ▼
   AWS Lambda (Node.js 22, Express + serverless-http)
          │
          │  Controller → Service → Repository (Clean Architecture)
          │
   ┌──────┴───────┐
   ▼              ▼
Amazon          Amazon S3
DynamoDB        (note images, with retry on upload failure)
(Notes table,
PAY_PER_REQUEST,
PITR enabled)

          ▲
          │  User authentication
   Amazon Cognito
   (User Pool + Hosted UI + Google Identity Provider)

          │
          ▼
   Amazon CloudWatch (Logs & error monitoring)
```

## 3.2. AWS Services Used

| Service | Role |
|---|---|
| **Amazon API Gateway** | Provides the REST API, applies Usage Plan/API Key and Cognito Authorizer |
| **AWS Lambda** | Handles Serverless business logic (Node.js 22, Express + serverless-http) |
| **Amazon DynamoDB** | Stores note data (PAY_PER_REQUEST mode) |
| **Amazon S3** | Stores images attached to notes |
| **Amazon Cognito** | User authentication (email + Google sign-up/sign-in), Hosted UI |
| **Amazon CloudWatch** | System monitoring, logging, and error lookup |
| **IAM** | Manages access permissions under the Least Privilege principle |
| **AWS SAM (CloudFormation)** | Infrastructure as Code — packages and deploys the entire infrastructure |
| **DynamoDB Point-in-Time Recovery (PITR)** | Continuous automatic backup, restores the Notes table to any point within the last 35 days |
| **CI/CD Pipeline** | Automates build & deploy (`sam build && sam deploy`) on every code change |

## 3.3. Component Design

**Frontend**
- Plain HTML/CSS/JS (a single static file, no build step required).
- "Library card catalog" style (Fraunces + Courier Prime fonts, "FILED" stamp motif).
- Calls the API directly via fetch, authenticates through the Cognito Hosted UI.

**Backend**
- Amazon API Gateway serves as the REST API entry point.
- AWS Lambda organized under Clean Architecture: Controller (receives requests) → Service (business logic) → Repository (data operations).

**Data Layer**
- Amazon DynamoDB: Notes table, primary key composed of `userId` + `noteId` to isolate data between users.
- Amazon S3: a dedicated bucket for note images, with image paths tied to `noteId`.

**Authentication**
- Amazon Cognito User Pool + Hosted UI.
- Google Identity Provider (sign-in with a Google account).

**Security & Throttling**
- Amazon API Gateway API Key + Usage Plan limits requests per second and requests per month.

**Monitoring**
- Amazon CloudWatch Logs for each Lambda, supporting error lookup and performance tracking.

---

# 4. Technical Implementation

## 4.1. Implementation Phases

| Phase | Content | Status |
|---|---|---|
| 1 | Architecture design & API definition (endpoints, response format, validation) | Completed |
| 2 | Deploy IAM, Amazon DynamoDB, and Amazon S3 (least-privilege) | Completed |
| 3 | Build Lambda under Clean Architecture (Controller/Service/Repository) + serverless-http | Completed |
| 4 | Deploy Amazon API Gateway, test CRUD + image upload/delete via Postman | Completed |
| 5 | Build the static HTML frontend, connect to the API via fetch/CORS | Completed |
| 6 | Add API Key + Usage Plan security (throttle/quota) | Completed |
| 7 | Integrate Amazon Cognito (Hosted UI + Google Identity Provider), enforce login | Completed |
| 8 | Set up CloudWatch Logs monitoring, error-lookup guide | Completed |
| 9 | Enable Point-in-Time Recovery (PITR) for DynamoDB, protecting data from accidental delete/edit | Completed |
| 10 | Set up CI/CD for automatic deployment (auto build & deploy on code push) | Completed |
| 11 | End-to-end system testing (CRUD, image upload, login/logout, security, PITR recovery) | Completed |

## 4.2. Technical Requirements

- AWS Account and an IAM User with appropriate permissions.
- Deployment region: **ap-southeast-1**.
- AWS CLI and AWS SAM CLI configured.
- Node.js version 22.
- Google Cloud Console — create an OAuth Client to support Google sign-in via Cognito.
- Postman — for manual API testing.
- Git — source code management and CI/CD triggering.

## 4.3. Testing Strategy

- **Functional testing**: note CRUD, image upload/delete, login/logout via Postman and the web interface.
- **Security testing**: verifying the API rejects requests missing an API Key or a valid Cognito token.
- **Data recovery testing**: simulating an accidental delete and restoring the Notes table using PITR.
- **Load testing (recommended addition)**: using tools such as Artillery or k6 to verify the auto-scaling behavior of Lambda/API Gateway under high load.

---

# 5. Timeline & Milestones

| Status | Task |
|---|---|
| ✅ Completed | Architecture design, CRUD + image upload built, successfully deployed |
| ✅ Completed | Static HTML frontend built (edit note, view note detail) |
| ✅ Completed | API Key + Usage Plan security |
| ✅ Completed | Cognito Hosted UI + Google sign-in integration |
| ✅ Completed | Point-in-Time Recovery (PITR) enabled for DynamoDB |
| ✅ Completed | CI/CD auto-deployment set up |
| ✅ Completed | CloudWatch log-lookup guide for errors |
| ✅ Completed | Load testing and performance evaluation under high traffic |
| ✅ Completed | Backend pagination/search, tagging, note pinning |

---

# 6. Cost Estimate

The **AWS Free Tier** can be used during development and testing. Refer to the official estimation tool at: https://calculator.aws

## 6.1. Estimated Infrastructure Cost (small-scale/personal testing)

| Service | Estimated cost/month | Notes |
|---|---|---|
| Amazon API Gateway | Free Tier | First 1 million requests free (first 12 months) |
| AWS Lambda | Free Tier | 1 million invocations + 400,000 GB-seconds free per month |
| Amazon DynamoDB | Free Tier | PAY_PER_REQUEST, minimal cost at small scale |
| Amazon S3 | ~$1–2 | Depends on storage volume and image retrieval requests |
| Amazon Cognito | Free Tier | Free for under 50,000 MAU (Monthly Active Users) |
| Amazon CloudWatch | ~$1–2 | Depends on log volume |

**Estimated total testing cost: approximately $2–5 USD/month.**

## 6.2. Cost Considerations When Scaling Up

Once Free Tier thresholds are exceeded (e.g., surpassing 50,000 MAU on Cognito, or a sharp increase in Lambda requests), costs will scale according to each service's pay-as-you-go model. It is recommended to set up a **Billing Alarm** (listed in section 7.2) to proactively track and control costs as the system gains a larger real-world user base beyond the testing phase.

---

# 7. Risk Assessment

## 7.1. Risk Matrix

| Risk | Likelihood | Impact | Mitigation status |
|---|---|---|---|
| API abuse/automated scanning before security is in place | Medium | High | Mitigated via API Key + Cognito Authorizer |
| Forgetting to delete AWS resources after use, incurring unexpected costs | Medium | Medium | Requires Billing Alarm setup |
| CORS/redirect URI misconfiguration when integrating Cognito + Google | Medium | Medium | Thoroughly tested before go-live |
| Data loss from accidental delete/edit | Low | High | Mitigated via PITR enabled on DynamoDB |
| API overload from concurrent high user traffic | Low | Medium | Lambda/API Gateway auto-scaling; load testing needed to confirm |
| Leakage of keys/sensitive information in source code or logs | Low | High | Requires periodic review, no hardcoded secrets |

## 7.2. Mitigation Strategy

- Use **Amazon CloudWatch** for real-time error monitoring and logging.
- Apply **IAM Least Privilege** to each Lambda, granting only the necessary permissions.
- Restrict API access with **API Key + Usage Plan + Cognito Authorizer**.
- Set up a **Billing Alarm** in AWS Budgets to track and alert on incurred costs.
- Enable **Point-in-Time Recovery (PITR)** on DynamoDB to restore data when needed.
- Deploy via **automated CI/CD** to reduce errors from manual deployment.
- Maintain a plan to delete all resources (`sam delete`) when no longer in use, to avoid unnecessary costs.

## 7.3. Contingency Plan

- **Retry** mechanism for failed S3 image uploads (already built into the Service layer).
- Error logs stored in CloudWatch for quick lookup and troubleshooting.
- The entire infrastructure can be restored at any time from `template.yaml` (Infrastructure as Code), ensuring fast environment reproducibility (disaster recovery).

---

# 8. Expected Outcomes

## 8.1. Technical Improvements

- A complete Serverless REST API built under Clean Architecture, easy to test and extend.
- Multi-layer security: API Key/Usage Plan at the gateway layer + Cognito Authorizer at the user authentication layer.
- Flexible login: email/password or Google account.
- The entire infrastructure managed via **Infrastructure as Code** (AWS SAM), ensuring reproducibility and version control for infrastructure.
- Proactive monitoring and error lookup via CloudWatch.
- Data protected via **Point-in-Time Recovery**, restorable within the last 35 days.
- Automated deployment process via **CI/CD**, reducing errors from manual deployment.

## 8.2. Long-Term Value

The Smart Notes API architecture is designed for reuse and can be extended to many similar CRUD use cases:

- A to-do list / personal task management app.
- A bookmark / link-saving app.
- A personal journal with image attachments.
- A team notes platform, if extended with user/group-based permission controls.
- Any small-scale CRUD API that needs fast, low-cost deployment on AWS.

Overall, this is a **lightweight, cost-optimized, easy-to-maintain Serverless solution** that meets the fundamental AWS architectural standards (security, resilience, scalability, and operational automation) for a personal/small-scale application, while providing a solid foundation for growth into a larger-scale product in the future.