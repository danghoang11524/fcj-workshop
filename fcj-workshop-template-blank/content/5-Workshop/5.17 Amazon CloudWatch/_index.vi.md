---
title : "Giám sát hệ thống với Amazon CloudWatch"
date : 2025-07-15
weight : 17
chapter : true
pre : " <b> 5.17. </b> "
---

# Giám sát hệ thống với Amazon CloudWatch

## Giới thiệu

Amazon CloudWatch là dịch vụ giám sát (Monitoring) và ghi nhật ký (Logging) của AWS.

Trong dự án **Smart Campus Guardian – AI Campus Incident Detection Platform**, Amazon CloudWatch được sử dụng để theo dõi toàn bộ hoạt động của hệ thống, bao gồm Logs, Metrics, Dashboard và Alarm.

CloudWatch giúp nhóm vận hành nhanh chóng phát hiện lỗi, theo dõi hiệu năng AI Workflow và nhận cảnh báo khi có sự cố xảy ra.

---

## Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ:

+ Theo dõi Logs của toàn bộ hệ thống.
+ Giám sát Metrics.
+ Tạo Dashboard.
+ Tạo Alarm khi có lỗi.
+ Kiểm tra AI Workflow.

---

# Kiến trúc Amazon CloudWatch

Amazon CloudWatch được kết nối với tất cả các dịch vụ trong hệ thống để thu thập Logs và Metrics.

![CloudWatch Architecture](/images/5-Workshop/5.17/5.17.1/cloudwatch-architecture.png)

---

## Giải thích kiến trúc

CloudWatch sẽ thu thập Logs và Metrics từ các dịch vụ sau:

+ Amazon API Gateway
+ AWS Lambda
+ Amazon EventBridge
+ AWS Step Functions
+ Amazon Rekognition
+ Amazon Bedrock
+ Amazon DynamoDB
+ Amazon SNS
+ Amazon SES

Sau khi thu thập dữ liệu, CloudWatch sẽ:

1. Ghi Logs của từng dịch vụ.
2. Thu thập Metrics.
3. Hiển thị Dashboard.
4. Kích hoạt Alarm khi phát hiện lỗi.

CloudWatch đóng vai trò trung tâm giám sát toàn bộ hệ thống Smart Campus Guardian.

---

# Các bước triển khai

## Bước 1: Mở Amazon CloudWatch

Đăng nhập **AWS Management Console**.

Tìm kiếm:

```
Amazon CloudWatch
```

---

## Bước 2: Kiểm tra Logs

Chọn:

```
Logs

↓

Log Groups
```

Bạn sẽ thấy Log Group của:

```
AWS Lambda

API Gateway

Step Functions

EventBridge
```

![Log Groups](/images/5-Workshop/5.17/5.17.2/log-groups.png)

---

## Bước 3: Tạo Dashboard

Chọn

```
Dashboards

↓

Create Dashboard
```

Tên Dashboard

```
SmartCampusGuardianDashboard
```

---

## Bước 4: Thêm Widget

Thêm các Metrics sau:

```
Lambda Invocations

Lambda Errors

API Gateway Requests

Step Functions Executions

DynamoDB Read Capacity

SNS Messages Published

SES Send Email

EventBridge Invocations
```

Dashboard sẽ hiển thị tình trạng hoạt động của toàn bộ hệ thống theo thời gian thực.

---

## Bước 5: Tạo Alarm

Chọn

```
Alarms

↓

Create Alarm
```

Metric

```
Lambda Errors
```

Condition

```
Greater than 5
```

Evaluation Period

```
5 Minutes
```

Action

```
Publish to SNS Topic

SmartCampusAlertTopic
```

Khi Lambda xảy ra lỗi vượt quá ngưỡng, CloudWatch sẽ tự động gửi cảnh báo.

---

## Bước 6: Kiểm tra

Upload hình ảnh:

```
fire.jpg
```

Sau khi AI Workflow hoàn thành:

```
CloudWatch

↓

Dashboard

↓

Logs

↓

Metrics

↓

Alarm
```

Kiểm tra Dashboard cập nhật dữ liệu và Logs của AI Workflow.

---

## Best Practices

✔ Thiết lập Log Retention.

✔ Theo dõi Metrics quan trọng.

✔ Tạo Dashboard riêng cho từng môi trường.

✔ Thiết lập Alarm cho Lambda, API Gateway và Step Functions.

✔ Sử dụng SNS để nhận cảnh báo.

---

## Kiểm tra

Sau khi hoàn thành chương này, bạn sẽ có:

+ Dashboard giám sát hệ thống.

+ Logs của tất cả dịch vụ.

+ Alarm hoạt động.

+ Metrics thời gian thực.

---

## Kết quả

Trong chương này, bạn đã triển khai thành công Amazon CloudWatch để giám sát toàn bộ hệ thống **Smart Campus Guardian**.

CloudWatch giúp theo dõi hiệu năng của AI Workflow, ghi nhận Logs, hiển thị Dashboard và gửi cảnh báo khi hệ thống gặp sự cố, hỗ trợ đội ngũ vận hành quản lý hệ thống một cách hiệu quả.

Trong chương tiếp theo, chúng ta sẽ triển khai **Frontend ReactJS** trên Amazon S3 và Amazon CloudFront để hoàn thiện hệ thống.