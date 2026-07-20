---
title : "Dashboard and Frontend"
date : 2025-07-15
weight : 18
chapter : true
pre : " <b> 5.18. </b> "
---

# Dashboard

## Introduction

The Dashboard is built with ReactJS and deployed on Amazon S3 with Amazon CloudFront.

The Dashboard uses Amazon Cognito for authentication and calls Amazon API Gateway to retrieve data.

---

## Features

+ Login

+ Dashboard

+ Camera

+ Incident

+ Alert History

+ AI Report

---

## Dashboard

Displays

```
Today's Incident

Camera Online

Camera Offline

High Risk

Recent Alert
```

---

## APIs

```
GET /dashboard

GET /incident

GET /camera
```

---

## Result

Administrators can monitor the status of the entire campus in real time.

![dashboard](/images/5-Workshop/5.18-Frontend/dashboard.png)
````
