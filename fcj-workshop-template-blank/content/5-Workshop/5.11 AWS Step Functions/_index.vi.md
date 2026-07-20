---
title : "Triển khai AWS Step Functions"
date : 2025-07-15
weight : 11
chapter : true
pre : " <b> 5.11. </b> "
---

# Triển khai AWS Step Functions

## Giới thiệu

AWS Step Functions là dịch vụ điều phối (Workflow Orchestration) của AWS, cho phép xây dựng các quy trình xử lý phức tạp bằng cách kết nối nhiều dịch vụ AWS theo dạng State Machine.

Trong dự án **Smart Campus Guardian – AI Campus Incident Detection Platform**, AWS Step Functions đóng vai trò điều phối toàn bộ quy trình phân tích AI sau khi Camera tải hình ảnh lên Amazon S3.

Thay vì để các Lambda Function gọi lẫn nhau, Step Functions quản lý toàn bộ luồng xử lý, giúp hệ thống dễ mở rộng, dễ giám sát và có khả năng tự động Retry khi xảy ra lỗi.

---

## Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ:

+ Tạo State Machine.
+ Xây dựng AI Workflow.
+ Thiết lập Retry Policy.
+ Thiết lập Error Handling.
+ Kết nối Lambda AI với Rekognition và Bedrock.

---

# Kiến trúc AWS Step Functions

AWS Step Functions là trung tâm điều phối AI Workflow của Smart Campus Guardian.

Sau khi EventBridge kích hoạt, Step Functions sẽ lần lượt gọi Lambda AI, Amazon Rekognition, Amazon Bedrock và các dịch vụ lưu trữ, thông báo.

![Step Functions Architecture](/images/5-Workshop/5.11/5.11.1/stepfunctions-architecture.png)

---

## Giải thích kiến trúc

Quy trình hoạt động như sau:

1. Amazon EventBridge kích hoạt State Machine.
2. AWS Step Functions gọi Lambda AI.
3. Lambda AI tải hình ảnh từ Amazon S3.
4. Lambda AI gọi Amazon Rekognition để nhận diện đối tượng.
5. Kết quả được gửi sang Amazon Bedrock để phân tích mức độ nguy hiểm.
6. Lambda AI ghi Incident vào Amazon DynamoDB.
7. Amazon SNS gửi thông báo.
8. Amazon SES gửi Email.
9. Amazon CloudWatch ghi Logs và Metrics.

Step Functions giúp toàn bộ quy trình được quản lý trực quan, dễ mở rộng và có khả năng tự động Retry khi có lỗi.

---

# Các bước triển khai

## Bước 1: Mở AWS Step Functions

Đăng nhập AWS Management Console.

Tìm kiếm:

```
AWS Step Functions
```

Chọn:

```
Create State Machine
```

---

## Bước 2: Tạo State Machine

Workflow Type

```
Standard
```

State Machine Name

```
SmartCampusAIWorkflow
```

![Create State Machine](/images/5-Workshop/5.11/5.11.2/state-machine.png)

---

## Bước 3: Thiết kế AI Workflow

State Machine gồm các bước:

```
Receive Image

↓

Lambda AI

↓

Amazon Rekognition

↓

Amazon Bedrock

↓

Lambda AI

↓

Amazon DynamoDB

↓

Amazon SNS

↓

Amazon SES

↓

Success
```

---

## Bước 4: Cấu hình Retry

Retry Count

```
3
```

Interval

```
5 Seconds
```

Backoff Rate

```
2
```

---

## Bước 5: Cấu hình Error Handling

Nếu Workflow xảy ra lỗi:

```
Catch

↓

CloudWatch Logs

↓

Execution Failed
```

CloudWatch sẽ ghi nhận toàn bộ thông tin để hỗ trợ việc kiểm tra và xử lý.

---

## Bước 6: Kiểm tra Workflow

Upload một hình ảnh:

```
fire.jpg
```

vào Bucket:

```
smart-campus-images
```

Kiểm tra:

```
AWS Step Functions

↓

Executions

↓

Succeeded
```

Nếu thành công, Incident sẽ được lưu vào Amazon DynamoDB và Email cảnh báo sẽ được gửi.

---

## Best Practices

✔ Thiết kế Workflow bằng State Machine.

✔ Không để Lambda gọi lẫn nhau.

✔ Thiết lập Retry Policy.

✔ Thiết lập Error Handling.

✔ Ghi Logs bằng Amazon CloudWatch.

✔ Tách biệt từng bước xử lý để dễ bảo trì.

---

## Kiểm tra

Sau khi hoàn thành chương này, bạn sẽ có:

+ AWS Step Functions State Machine.

+ AI Workflow hoàn chỉnh.

+ Retry Policy.

+ Error Handling.

+ CloudWatch Logs.

+ Workflow sẵn sàng tích hợp với Amazon Rekognition và Amazon Bedrock.

---

## Kết quả

Trong chương này, bạn đã triển khai thành công AWS Step Functions để điều phối toàn bộ AI Workflow của hệ thống **Smart Campus Guardian**.

AWS Step Functions giúp kết nối Lambda AI, Amazon Rekognition, Amazon Bedrock, DynamoDB, SNS và SES thành một quy trình xử lý tự động, ổn định và dễ dàng mở rộng.

Trong chương tiếp theo, chúng ta sẽ triển khai **Amazon Rekognition** để xây dựng chức năng nhận diện đối tượng trong hình ảnh.