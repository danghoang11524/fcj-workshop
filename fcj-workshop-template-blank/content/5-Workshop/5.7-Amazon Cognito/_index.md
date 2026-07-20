---
title : "Deploy Amazon Cognito"
date : 2025-07-14
weight : 7
chapter : true
pre : " <b> 5.7. </b> "
---

# Deploy Amazon Cognito

## Introduction

Amazon Cognito is AWS's Identity Management service that enables you to build a secure user authentication system without implementing your own authentication mechanism.

In the **Smart Campus Guardian – AI Campus Incident Detection Platform** project, Amazon Cognito is used to manage administrator accounts, authenticate user sign-in, and issue **JWT Tokens** for the React web application. These tokens are then used to authorize requests sent through Amazon API Gateway.

---

## Objectives

After completing this chapter, you will be able to:

+ Create an Amazon Cognito User Pool.
+ Create an App Client.
+ Create an administrator account.
+ Configure a Password Policy.
+ Sign in and obtain JWT Tokens.
+ Prepare authentication for Amazon API Gateway.

---

# Amazon Cognito Architecture

Amazon Cognito is responsible for authenticating users before they access the Smart Campus Guardian APIs.

After a successful sign-in, Cognito issues a JWT Token that Amazon API Gateway verifies before forwarding requests to AWS Lambda.

![Cognito Architecture](/images/5-Workshop/5.7/5.7.1/cognito-architecture.png)

---

## Architecture Overview

The authentication flow works as follows:

1. The user accesses the React website through Amazon CloudFront.
2. The website displays the sign-in page.
3. Amazon Cognito authenticates the user's credentials.
4. After successful authentication, Cognito issues:
   - Access Token
   - ID Token
   - Refresh Token
5. The React application uses the Access Token to send requests to Amazon API Gateway.
6. Amazon API Gateway validates the JWT Token before allowing access to backend APIs.

Using Amazon Cognito enhances system security, reduces development effort, and provides a scalable authentication solution.

---

# Deployment Steps

## Step 1: Open Amazon Cognito

Sign in to the **AWS Management Console**.

Search for:

```
Amazon Cognito
```

Select:

```
Create User Pool
```

![Create User Pool](/images/5-Workshop/5.7/5.7.2/create-userpool.png)

---

## Step 2: Configure the Sign-in Method

Under **Sign-in Options**, choose:

```
Email
```

Authentication Method

```
Username + Password
```

This configuration allows users to sign in using their email addresses.

---

## Step 3: Configure the Password Policy

Configure the password policy as follows:

Minimum Length

```
8
```

Requirements:

✔ Uppercase Letter

✔ Lowercase Letter

✔ Number

✔ Special Character

A strong password policy helps improve the overall security of the system.

---

## Step 4: Create a User Pool

User Pool Name

```
SmartCampusGuardianUserPool
```

This User Pool stores all user accounts for the system.

---

## Step 5: Create an App Client

Select:

```
Create App Client
```

App Client Name

```
SmartCampusGuardianWeb
```

Authentication Flow

```
ALLOW_USER_PASSWORD_AUTH
```

```
ALLOW_REFRESH_TOKEN_AUTH
```

Callback URL

```
https://xxxxxxxx.cloudfront.net
```

For this workshop, you can use the CloudFront domain created in the previous chapter.

---

## Step 6: Create an Administrator Account

Select:

```
Create User
```

User Information

Email

```
admin@campus.local
```

Password

```
Admin@123456
```

This account will be used to access the administration dashboard.

---

## Step 7: Verify the Sign-in Process

Sign in using the administrator account you just created.

After a successful sign-in, Amazon Cognito returns:

+ Access Token

+ ID Token

+ Refresh Token

The React application will use the **Access Token** to call Amazon API Gateway in the following chapters.

---

## Best Practices

To improve the security of your system, follow these recommendations:

✔ Enable Email Verification.

✔ Use a strong Password Policy.

✔ Enable Multi-Factor Authentication (MFA) in production environments.

✔ Avoid storing JWT Tokens in Local Storage when handling sensitive data.

✔ Use Amazon Cognito instead of building a custom authentication system.

---

## Verification

After completing this chapter, you will have:

+ An Amazon Cognito User Pool.

+ An App Client.

+ An administrator account.

+ A JWT Access Token.

+ Authentication ready for integration with Amazon API Gateway.

---

## Result

In this chapter, you successfully deployed Amazon Cognito to manage users and authenticate access for the **Smart Campus Guardian** system.

Amazon Cognito securely issues JWT Tokens, simplifies user management, and integrates seamlessly with Amazon API Gateway without requiring a custom authentication solution.

In the next chapter, you will deploy **Amazon API Gateway** to build REST APIs and connect them with AWS Lambda.