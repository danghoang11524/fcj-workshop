---
title : "Deploy Amazon CloudFront"
date : 2025-07-14
weight : 6
chapter : true
pre : " <b> 5.6. </b> "
---

# Deploy Amazon CloudFront

## Introduction

Amazon CloudFront is AWS's **Content Delivery Network (CDN)** service that delivers content to users through a global network of **Edge Locations**.

In the **Smart Campus Guardian – AI Campus Incident Detection Platform** project, CloudFront is used to distribute the React website hosted on Amazon S3, providing faster access, HTTPS support, and enhanced security.

---

## Objectives

After completing this chapter, you will be able to:

- Create a CloudFront Distribution.
- Connect CloudFront to Amazon S3.
- Configure Origin Access Control (OAC).
- Enable HTTPS for the website.
- Distribute the React website globally.

---

# CloudFront Architecture

CloudFront is the first layer that users interact with when accessing the Smart Campus Guardian system.

CloudFront receives user requests and delivers content from Amazon S3 through the nearest Edge Location, reducing latency and improving performance.

![CloudFront Architecture](/images/5-Workshop/5.6/5.6.1/cloudfront-architecture.png)

---

## Architecture Overview

The CloudFront workflow is as follows:

1. A user accesses the website.
2. CloudFront checks whether the requested content exists in the Edge Cache.
3. If the content is cached, CloudFront returns it immediately.
4. Otherwise, CloudFront retrieves the content from Amazon S3.
5. The content is cached at the Edge Location for future requests.

Using CloudFront provides several benefits:

- Faster website loading times.
- HTTPS support.
- Reduced load on Amazon S3.
- Improved security through Origin Access Control (OAC).

---

# Deployment Steps

## Step 1: Open Amazon CloudFront

Sign in to the **AWS Management Console**.

Search for:

```
CloudFront
```

Select:

```
Create Distribution
```

---

## Step 2: Configure the Origin

Origin Domain

```
smart-campus-frontend
```

Origin Type

```
Amazon S3
```

Origin Access

```
Origin Access Control (OAC)
```

CloudFront will use OAC to securely access the Amazon S3 bucket without requiring it to be publicly accessible.

![Create Distribution](/images/5-Workshop/5.6/5.6.2/create-distribution.png)

---

## Step 3: Configure the Viewer

Viewer Protocol Policy

```
Redirect HTTP to HTTPS
```

Default Root Object

```
index.html
```

Price Class

```
Use All Edge Locations
```

These settings ensure that the website always uses HTTPS and can be accessed efficiently from anywhere in the world.

---

## Step 4: Create the Distribution

Review all configuration settings.

Select:

```
Create Distribution
```

The deployment process typically takes **10–15 minutes**.

---

## Step 5: Verify CloudFront

Once the distribution has been created successfully, CloudFront provides a domain name.

Example:

```
d123abcxyz.cloudfront.net
```

Open your browser and visit the domain.

If the React website loads successfully, the deployment is complete.

---

## Best Practices

To maximize performance and security, follow these recommendations:

✔ Use HTTPS across the entire website.

✔ Use Origin Access Control (OAC).

✔ Do not make the Amazon S3 bucket public.

✔ Enable Compression to reduce data transfer size.

✔ Use CloudFront's default Cache Policy.

✔ Use AWS Certificate Manager when deploying a custom domain.

---

## Verification

After completing this chapter, you should have:

- A working CloudFront Distribution.

- The React website delivered through CloudFront.

- HTTPS enabled.

- Amazon S3 accessible only through CloudFront using OAC.

---

## Result

In this chapter, you successfully deployed Amazon CloudFront to distribute the Smart Campus Guardian React website.

CloudFront acts as the CDN layer in front of Amazon S3, improving website performance, reducing latency, and enhancing the overall security of the system.

In the next chapter, we will deploy **Amazon Cognito** to manage users and authenticate access before calling the system APIs.