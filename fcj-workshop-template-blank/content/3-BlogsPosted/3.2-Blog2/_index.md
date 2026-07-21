---
title: "Blog 2"
date: 2026-07-21
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# From an Open API to Mandatory Authentication: The Journey of Securing Smart Notes API

In the previous blog, I shared how I built the Smart Notes REST API using a Serverless architecture. After deploying the application and successfully testing the CRUD operations, I realized a fairly obvious but often overlooked issue: the API was **completely public**—anyone with the URL could access it without any restrictions. In this article, I'll share the two security layers I added to solve this problem.

## Layer 1: API Key + Usage Plan

The first and simplest step was enabling an API Key at the API Gateway level. In `template.yaml`, I only needed to add:

```yaml
SmartNotesApi:
  Type: AWS::Serverless::Api
  Properties:
    Auth:
      ApiKeyRequired: true
```

Along with that, I configured a Usage Plan to limit the request rate (10 requests per second) and quota (5,000 requests per month). This helps block random bots, reduce abuse, and avoid unexpected AWS costs.

One important lesson from this step is that **an API Key is not user authentication**.

Since the frontend is just a static HTML file, anyone can inspect the source code or browser Developer Tools to find the key. It only prevents casual access and limits request rates—it cannot identify **who** is making the request.

## Layer 2: Cognito Hosted UI + Google Sign-In

Because an API Key alone wasn't enough, I integrated **Amazon Cognito** to require users to authenticate before accessing their notes.

I chose **Amazon Cognito Hosted UI**, AWS's built-in sign-in/sign-up page, which supports:

- Email and password sign-up/sign-in
- Google Sign-In (through Google Identity Provider)

There was no need to build a custom login page. I only had to configure a User Pool, Hosted UI domain, and connect a Google OAuth Client (created in Google Cloud Console) inside `template.yaml`.

On API Gateway, I attached a Cognito Authorizer—but only for the `/notes*` routes. The static HTML page remained publicly accessible. Otherwise, users wouldn't even be able to load the page to click the login button—a circular dependency I almost ran into.

```yaml
Auth:
  ApiKeyRequired: true
  Authorizers:
    CognitoAuthorizer:
      UserPoolArn: !GetAtt SmartNotesUserPool.Arn
  DefaultAuthorizer: CognitoAuthorizer
```

After users sign in through the Hosted UI, Cognito redirects them back with an `id_token` in the URL.

The frontend stores this token in `localStorage`, sends it in the `Authorization` header for every request to `/notes`, and automatically redirects users back to the login page when the token expires.

## A Few Challenges Along the Way

- The Redirect URI must match **exactly** across Google Cloud Console, the Cognito App Client, and the frontend. Even an extra or missing trailing slash causes authentication to fail.
- Google requires configuring the OAuth Consent Screen (Audience/Test Users) before an OAuth Client can be created—an easy step to miss if following outdated tutorials.
- Since the Google Client Secret is defined using the `NoEcho` parameter, AWS SAM CLI doesn't save it. As a result, it must be entered manually or passed through `--parameter-overrides` during every deployment (which becomes especially important for CI/CD in the next blog).

## Results

With these two security layers in place, Smart Notes API is now protected against anonymous access and abuse (API Key), while also requiring users to authenticate (Amazon Cognito + Google Sign-In) before they can access or manage their notes.

In the next blog, I'll discuss the operational side of the project: automating deployment with CI/CD and protecting data using Point-in-Time Recovery (PITR).

**Read More:**

- **Blog 1:** Building Smart Notes API
- **Blog 3:** Automated CI/CD and Data Protection with PITR
- **Original post on the AWS Study Group FCJ Facebook community**
- **Source code on GitHub**