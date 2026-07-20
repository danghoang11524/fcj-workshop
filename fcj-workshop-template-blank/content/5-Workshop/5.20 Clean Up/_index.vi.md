---
title : "Dọn dẹp tài nguyên"
date : 2025-07-15
weight : 20
chapter : true
pre : " <b> 5.20. </b> "
---

# Dọn dẹp tài nguyên

## Giới thiệu

Sau khi hoàn thành Workshop **Smart Campus Guardian**, bạn nên xóa toàn bộ tài nguyên AWS đã tạo để tránh phát sinh chi phí không cần thiết.

AWS khuyến nghị thực hiện theo đúng thứ tự để tránh lỗi do các dịch vụ còn phụ thuộc lẫn nhau.

---

# Mục tiêu

Sau khi hoàn thành chương này bạn sẽ:

+ Xóa toàn bộ hạ tầng AWS.
+ Đảm bảo không còn dịch vụ đang chạy.
+ Kiểm tra chi phí trên AWS Billing.
+ Kết thúc Workshop an toàn.

---

# Kiến trúc dọn dẹp

Việc dọn dẹp sẽ được thực hiện theo chiều ngược lại so với kiến trúc triển khai.

![Cleanup Architecture](/images/5-Workshop/5.20/5.20.1/cleanup-architecture.png)

---

# Thứ tự dọn dẹp

## Bước 1

Xóa Amazon CloudFront Distribution

```
CloudFront

↓

Disable Distribution

↓

Delete Distribution
```

Đợi Distribution chuyển sang trạng thái **Deleted** trước khi tiếp tục.

---

## Bước 2

Xóa Website trên Amazon S3

Bucket:

```
smart-campus-frontend
```

Xóa toàn bộ Object.

Sau đó Delete Bucket.

---

## Bước 3

Xóa Bucket chứa ảnh Camera

Bucket:

```
smart-campus-images
```

Xóa toàn bộ Object.

Sau đó Delete Bucket.

---

## Bước 4

Xóa Amazon API Gateway

```
SmartCampusGuardianAPI
```

↓

Delete API

---

## Bước 5

Xóa AWS Lambda

Xóa toàn bộ Lambda Function:

```
DashboardFunction

IncidentFunction

CameraFunction

AIWorkflowFunction
```

---

## Bước 6

Xóa AWS Step Functions

```
SmartCampusWorkflow
```

↓

Delete State Machine

---

## Bước 7

Xóa Amazon EventBridge Rule

```
SmartCampusImageUploaded
```

↓

Delete Rule

---

## Bước 8

Xóa Amazon SNS

```
CampusAlertTopic
```

↓

Delete Topic

---

## Bước 9

Xóa Amazon SES

Vào

```
Verified Identities
```

↓

Delete Identity

---

## Bước 10

Xóa Amazon DynamoDB

Table

```
Incident
```

↓

Delete Table

---

## Bước 11

Xóa Amazon Cognito

```
SmartCampusGuardianUserPool
```

↓

Delete User Pool

---

## Bước 12

Xóa IAM Role

```
SmartCampusLambdaRole

SmartCampusStepFunctionRole
```

↓

Delete Role

---

## Bước 13

Kiểm tra CloudWatch

Xóa:

+ Log Groups

+ Dashboard

+ Alarm

(nếu đã tạo trong Workshop)

---

## Bước 14

Kiểm tra AWS Billing

Truy cập

```
AWS Billing

↓

Bills
```

Đảm bảo:

✔ Không còn dịch vụ đang hoạt động.

✔ Không còn phát sinh chi phí.

---

## Best Practices

✔ Xóa theo đúng thứ tự.

✔ Xóa Object trước khi xóa S3 Bucket.

✔ Kiểm tra CloudWatch Log Groups.

✔ Kiểm tra AWS Billing sau khi hoàn thành.

✔ Không để tài nguyên chạy sau Workshop.

---

## Kết quả

Sau khi hoàn thành chương này, toàn bộ hạ tầng AWS của Smart Campus Guardian đã được dọn dẹp hoàn toàn.

Workshop kết thúc và tài khoản AWS không còn phát sinh chi phí từ các tài nguyên đã triển khai.

![Cleanup Result](/images/5-Workshop/5.20/5.20.2/cleanup-result.png)