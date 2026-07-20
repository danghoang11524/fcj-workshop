---
title : "Triển khai Amazon EventBridge"
date : 2025-07-14
weight : 10
chapter : true
pre : " <b> 5.10. </b> "
---

# Triển khai Amazon EventBridge

## Giới thiệu

Amazon EventBridge là dịch vụ **Event Bus** của AWS, cho phép các dịch vụ trong hệ thống giao tiếp với nhau theo mô hình **Event-Driven Architecture**.

Trong dự án **Smart Campus Guardian – AI Campus Incident Detection Platform**, EventBridge chịu trách nhiệm tiếp nhận sự kiện khi Camera tải hình ảnh lên Amazon S3 và tự động kích hoạt AI Workflow thông qua AWS Step Functions.

Việc sử dụng EventBridge giúp hệ thống giảm sự phụ thuộc giữa các dịch vụ, dễ dàng mở rộng và xử lý đồng thời nhiều Camera.

---

## Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ:

+ Tạo EventBridge Rule.
+ Theo dõi sự kiện Object Created từ Amazon S3.
+ Kích hoạt AWS Step Functions tự động.
+ Kiểm tra Event được xử lý thành công.
+ Chuẩn bị AI Workflow cho các chương tiếp theo.

---

# Kiến trúc Amazon EventBridge

Amazon EventBridge là trung tâm điều phối sự kiện của hệ thống Smart Campus Guardian.

Khi Camera tải hình ảnh lên Amazon S3, EventBridge sẽ nhận sự kiện và kích hoạt AWS Step Functions để bắt đầu quy trình phân tích AI.

![EventBridge Architecture](/images/5-Workshop/5.10/5.10.1/eventbridge-architecture.png)

---

## Giải thích kiến trúc

Quy trình hoạt động như sau:

1. Camera tải hình ảnh lên Amazon S3.
2. Amazon S3 phát sinh sự kiện **Object Created**.
3. Amazon EventBridge nhận sự kiện.
4. EventBridge kiểm tra Rule phù hợp.
5. EventBridge kích hoạt AWS Step Functions.
6. Step Functions bắt đầu AI Workflow.

Nhờ EventBridge, Amazon S3 không cần gọi trực tiếp Lambda, giúp hệ thống linh hoạt và dễ dàng mở rộng khi bổ sung thêm nhiều quy trình xử lý khác.

---

# Các bước triển khai

## Bước 1: Mở Amazon EventBridge

Đăng nhập **AWS Management Console**.

Tìm kiếm:

```
Amazon EventBridge
```

Chọn:

```
Rules
```

↓

```
Create Rule
```

---

## Bước 2: Tạo Rule

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

## Bước 3: Cấu hình Event Pattern

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

Rule này sẽ theo dõi mọi hình ảnh mới được tải lên Bucket.

---

## Bước 4: Cấu hình Target

Target

```
AWS Step Functions State Machine
```

State Machine

```
SmartCampusAIWorkflow
```

Mỗi khi Event xảy ra, EventBridge sẽ tự động khởi động State Machine.

---

## Bước 5: Tạo Rule

Kiểm tra lại cấu hình.

Chọn:

```
Create Rule
```

Sau khi hoàn thành, Rule sẽ ở trạng thái:

```
Enabled
```

---

## Bước 6: Kiểm tra Event

Upload thử một hình ảnh:

```
fire.jpg
```

vào Bucket

```
smart-campus-images
```

Sau vài giây, kiểm tra:

```
Amazon EventBridge

↓

Monitoring

↓

Invocations
```

Nếu Rule hoạt động đúng, số lượng **Invocations** sẽ tăng lên.

Đồng thời, AWS Step Functions sẽ được kích hoạt.

---

## Best Practices

Để đảm bảo khả năng mở rộng và bảo trì, nên áp dụng các khuyến nghị sau:

✔ Không để Amazon S3 gọi trực tiếp Lambda.

✔ Sử dụng EventBridge làm Event Bus trung tâm.

✔ Thiết kế theo Event-Driven Architecture.

✔ Một Event có thể kích hoạt nhiều Workflow khác nhau.

✔ Ghi nhận Logs và Metrics bằng Amazon CloudWatch.

---

## Kiểm tra

Sau khi hoàn thành chương này, bạn sẽ có:

+ EventBridge Rule.

+ Event Pattern theo dõi Amazon S3.

+ Rule ở trạng thái Enabled.

+ AWS Step Functions được kích hoạt tự động.

+ Event sẵn sàng cho AI Workflow.

---

## Kết quả

Trong chương này, bạn đã triển khai thành công Amazon EventBridge để điều phối các sự kiện trong hệ thống **Smart Campus Guardian**.

Amazon EventBridge giúp kết nối Amazon S3 với AWS Step Functions theo mô hình Event-Driven, tạo nền tảng cho quy trình phân tích AI tự động.

Trong chương tiếp theo, chúng ta sẽ triển khai **AWS Step Functions** để xây dựng toàn bộ quy trình AI Workflow của hệ thống.