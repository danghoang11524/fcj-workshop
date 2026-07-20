---
title : "Kiểm thử hệ thống"
date : 2025-07-15
weight : 19
chapter : true
pre : " <b> 5.19. </b> "
---

# Kiểm thử hệ thống

## Giới thiệu

Sau khi hoàn thành việc triển khai toàn bộ hạ tầng AWS, chúng ta sẽ tiến hành kiểm thử toàn bộ hệ thống **Smart Campus Guardian – AI Campus Incident Detection Platform**.

Mục tiêu của chương này là xác nhận toàn bộ AI Workflow hoạt động chính xác từ lúc người dùng truy cập Dashboard cho đến khi hệ thống phát hiện sự cố, phân tích AI và gửi cảnh báo.

---

# Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ có thể:

+ Kiểm tra Dashboard.
+ Kiểm tra Authentication.
+ Kiểm tra API Gateway.
+ Kiểm tra AI Workflow.
+ Kiểm tra Notification.
+ Kiểm tra Monitoring.

---

# Kiến trúc kiểm thử

Quy trình kiểm thử sẽ đi theo đúng kiến trúc của hệ thống.

![Testing Architecture](/images/5-Workshop/5.19/5.19.1/testing-architecture.png)

---

## Giải thích quy trình

Quy trình kiểm thử bao gồm:

1. Người quản trị đăng nhập Dashboard.
2. Dashboard gọi API Gateway.
3. Lambda xử lý Request.
4. Camera upload ảnh lên Amazon S3.
5. EventBridge nhận sự kiện.
6. Step Functions khởi động AI Workflow.
7. Lambda AI gọi Amazon Rekognition.
8. Amazon Bedrock đánh giá mức độ rủi ro.
9. Lambda AI lưu Incident vào DynamoDB.
10. Lambda AI gửi Notification qua SNS và SES.
11. CloudWatch ghi Logs và Metrics.

---

# Test Case 1

## Đăng nhập Dashboard

Truy cập:

```
https://xxxxxxxx.cloudfront.net
```

Đăng nhập bằng tài khoản Amazon Cognito.

Kết quả mong đợi

✔ Login thành công

✔ Nhận JWT Token

✔ Truy cập Dashboard

---

# Test Case 2

## Kiểm tra API Gateway

Mở Dashboard.

Dashboard gọi:

```
GET /dashboard

GET /incident

GET /camera
```

Kết quả mong đợi

✔ HTTP 200

✔ Có dữ liệu trả về

---

# Test Case 3

## Upload Image

Upload

```
fire.jpg
```

vào Bucket

```
smart-campus-images
```

Kết quả mong đợi

✔ Upload thành công

✔ Amazon EventBridge nhận Event

---

# Test Case 4

## Kiểm tra AI Workflow

Mở

```
AWS Step Functions
```

↓

```
Executions
```

Kết quả mong đợi

```
Succeeded
```

---

# Test Case 5

## Amazon Rekognition

Kiểm tra CloudWatch Logs.

Kết quả mong đợi

```
Fire

Smoke

Person
```

được phát hiện.

---

# Test Case 6

## Amazon Bedrock

Kiểm tra kết quả AI.

Ví dụ

```
Risk Level

HIGH

Summary

Fire detected near Building A

Recommended Action

Notify campus security immediately.
```

---

# Test Case 7

## Amazon DynamoDB

Mở

```
Amazon DynamoDB

↓

Incident
```

Kết quả mong đợi

Có Incident mới.

---

# Test Case 8

## Amazon SNS

Mở

```
Amazon SNS

↓

Topic

↓

Metrics
```

Kiểm tra

```
Messages Published
```

Kết quả mong đợi

Lớn hơn 0.

---

# Test Case 9

## Amazon SES

Kiểm tra Email

```
security@school.edu
```

Kết quả mong đợi

Nhận Email cảnh báo.

---

# Test Case 10

## Dashboard

Dashboard hiển thị:

+ Today's Incident

+ High Risk Alert

+ AI Report

+ Camera Status

Dữ liệu phải trùng với DynamoDB.

---

# Test Case 11

## Amazon CloudWatch

Kiểm tra:

+ Lambda Logs

+ API Gateway Logs

+ Step Functions Logs

+ Metrics

+ Dashboard

Kết quả mong đợi

Không có lỗi.

---

# Kết quả

Sau khi hoàn thành chương này, toàn bộ hệ thống Smart Campus Guardian đã hoạt động đúng theo kiến trúc đã thiết kế.

Dashboard hiển thị dữ liệu theo thời gian thực, AI Workflow xử lý thành công, Notification được gửi và CloudWatch ghi nhận đầy đủ Logs cùng Metrics.

![Testing Result](/images/5-Workshop/5.19/5.19.2/testing-result.png)