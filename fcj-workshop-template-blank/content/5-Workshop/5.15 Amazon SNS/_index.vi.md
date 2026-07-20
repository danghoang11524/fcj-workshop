---
title : "Triển khai Amazon SNS"
date : 2025-07-15
weight : 15
chapter : true
pre : " <b> 5.15. </b> "
---

# Triển khai Amazon SNS

## Giới thiệu

Amazon Simple Notification Service (Amazon SNS) là dịch vụ nhắn tin theo mô hình Publish/Subscribe (Pub/Sub) của AWS.

Trong dự án **Smart Campus Guardian – AI Campus Incident Detection Platform**, Amazon SNS được sử dụng để phát tán thông báo khi hệ thống phát hiện sự cố có mức độ **HIGH** hoặc **CRITICAL**.

Sau khi Amazon Bedrock hoàn thành phân tích và Lambda AI lưu Incident vào Amazon DynamoDB, Lambda AI sẽ Publish một thông báo đến SNS Topic. Các Subscriber sẽ nhận được thông báo theo thời gian thực.

---

## Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ:

+ Tạo SNS Topic.
+ Tạo Subscription.
+ Publish Message từ Lambda AI.
+ Kiểm tra thông báo được gửi thành công.

---

# Kiến trúc Amazon SNS

Amazon SNS là dịch vụ gửi thông báo theo thời gian thực trong AI Workflow.

Sau khi Incident được lưu vào Amazon DynamoDB, Lambda AI sẽ Publish Message đến SNS Topic để thông báo cho các hệ thống hoặc dịch vụ đăng ký.

![SNS Architecture](/images/5-Workshop/5.15/5.15.1/sns-architecture.png)

---

## Giải thích kiến trúc

Quy trình hoạt động như sau:

1. AWS Step Functions điều phối AI Workflow.
2. Lambda AI nhận kết quả từ Amazon Bedrock.
3. Lambda AI lưu Incident vào Amazon DynamoDB.
4. Lambda AI Publish Message đến Amazon SNS.
5. Amazon SNS phân phối thông báo đến các Subscriber.
6. Amazon CloudWatch ghi Logs và Metrics.

Amazon SNS giúp tách biệt việc xử lý AI với việc gửi thông báo, giúp hệ thống dễ mở rộng và dễ tích hợp thêm các dịch vụ khác.

---

# Các bước triển khai

## Bước 1: Mở Amazon SNS

Đăng nhập **AWS Management Console**.

Tìm kiếm:

```
Amazon SNS
```

Chọn:

```
Topics
```

↓

```
Create Topic
```

---

## Bước 2: Tạo Topic

Topic Type

```
Standard
```

Topic Name

```
SmartCampusAlertTopic
```

Display Name

```
Campus Alert
```

![Create SNS Topic](/images/5-Workshop/5.15/5.15.2/create-topic.png)

---

## Bước 3: Tạo Subscription

Chọn Topic vừa tạo.

↓

```
Create Subscription
```

Protocol

```
Email
```

Endpoint

```
security@school.edu
```

> Trong môi trường Production, bạn có thể sử dụng thêm SMS, AWS Lambda hoặc HTTP Endpoint làm Subscriber.

---

## Bước 4: Cập nhật Lambda AI

Sau khi Incident được lưu vào Amazon DynamoDB, Lambda AI sẽ sử dụng API:

```
Publish
```

để gửi thông báo đến Topic:

```
SmartCampusAlertTopic
```

Ví dụ nội dung:

```text
Incident Detected

Location: Building A

Risk Level: HIGH

Summary:
Fire detected near Building A.

Time:
2025-07-15 10:30 UTC
```

---

## Bước 5: Kiểm tra

Upload hình ảnh:

```
fire.jpg
```

Sau khi AI Workflow hoàn thành:

```
Lambda AI

↓

Amazon SNS

↓

Topic

↓

Messages Published
```

Kiểm tra số lượng Message Published lớn hơn 0.

---

## Best Practices

✔ Chỉ Publish khi Risk Level là HIGH hoặc CRITICAL.

✔ Không gửi dữ liệu nhạy cảm trong Message.

✔ Thiết kế Topic theo từng nhóm thông báo.

✔ Sử dụng IAM Role thay vì Access Key.

✔ Ghi Logs bằng Amazon CloudWatch.

---

## Kiểm tra

Sau khi hoàn thành chương này, bạn sẽ có:

+ Amazon SNS Topic.

+ Subscription hoạt động.

+ Lambda AI Publish Message thành công.

+ Thông báo thời gian thực.

---

## Kết quả

Trong chương này, bạn đã triển khai thành công Amazon SNS để gửi thông báo theo thời gian thực trong hệ thống **Smart Campus Guardian**.

Amazon SNS giúp phát tán thông báo đến các Subscriber ngay sau khi AI phát hiện sự cố có mức độ nguy hiểm cao, đồng thời giữ cho AI Workflow tách biệt với các dịch vụ thông báo.

Trong chương tiếp theo, chúng ta sẽ triển khai **Amazon SES** để gửi Email cảnh báo chính thức đến bộ phận quản lý và đội ngũ bảo vệ.