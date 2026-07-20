---
title : "Triển khai AWS Lambda"
date : 2025-07-14
weight : 9
chapter : true
pre : " <b> 5.9. </b> "
---

# Triển khai AWS Lambda

## Giới thiệu

AWS Lambda là dịch vụ **Serverless Compute** của AWS, cho phép thực thi mã nguồn mà không cần quản lý máy chủ.

Trong dự án **Smart Campus Guardian – AI Campus Incident Detection Platform**, AWS Lambda là thành phần xử lý nghiệp vụ trung tâm của hệ thống.

Lambda nhận yêu cầu từ Amazon API Gateway để xử lý các API của Website React, đồng thời xử lý các sự kiện từ AI Workflow khi Amazon EventBridge kích hoạt.

---

## Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ:

+ Tạo AWS Lambda Function.
+ Cấu hình Runtime và IAM Role.
+ Thiết lập Environment Variables.
+ Kết nối Lambda với Amazon API Gateway.
+ Chuẩn bị Lambda cho AI Workflow.
+ Ghi Logs lên Amazon CloudWatch.

---

# Kiến trúc AWS Lambda

AWS Lambda là tầng xử lý nghiệp vụ của hệ thống Smart Campus Guardian.

Lambda nhận yêu cầu từ API Gateway hoặc EventBridge, sau đó xử lý dữ liệu và tương tác với DynamoDB, SNS, SES cũng như các dịch vụ AI của AWS.

![Lambda Architecture](/images/5-Workshop/5.9/5.9.1/lambda-architecture.png)

---

## Giải thích kiến trúc

Lambda hoạt động theo hai luồng chính.

### API Workflow

1. Website React gửi yêu cầu đến Amazon API Gateway.
2. API Gateway xác thực JWT Token.
3. API Gateway gọi AWS Lambda.
4. Lambda xử lý nghiệp vụ.
5. Lambda đọc hoặc ghi dữ liệu vào Amazon DynamoDB.
6. Kết quả được trả về Website.

### AI Workflow

1. Amazon EventBridge nhận sự kiện từ Amazon S3.
2. EventBridge kích hoạt AWS Step Functions.
3. Step Functions gọi AWS Lambda để tiền xử lý dữ liệu.
4. Lambda chuyển hình ảnh sang Amazon Rekognition.
5. Kết quả tiếp tục được xử lý bởi Amazon Bedrock.

AWS Lambda đóng vai trò là cầu nối giữa các dịch vụ trong toàn bộ hệ thống.

---

# Các bước triển khai

## Bước 1: Mở AWS Lambda

Đăng nhập **AWS Management Console**.

Tìm kiếm:

```
AWS Lambda
```

Chọn:

```
Create Function
```

---

## Bước 2: Tạo Lambda Function

Function Name

```
SmartCampusHandler
```

Runtime

```
Python 3.12
```

Architecture

```
x86_64
```

Execution Role

```
SmartCampusLambdaRole
```

![Create Lambda](/images/5-Workshop/5.9/5.9.2/create-lambda.png)

---

## Bước 3: Cấu hình Runtime

Memory

```
512 MB
```

Timeout

```
30 Seconds
```

Ephemeral Storage

```
512 MB
```

Các thông số này phù hợp với khối lượng xử lý của Workshop.

---

## Bước 4: Cấu hình Environment Variables

Thêm các biến môi trường:

```
IMAGE_BUCKET

REPORT_BUCKET

INCIDENT_TABLE

SNS_TOPIC_ARN

AWS_REGION
```

Lambda sẽ sử dụng các biến này để kết nối với các dịch vụ AWS mà không cần hard-code.

---

## Bước 5: Kết nối API Gateway

Trigger

```
Amazon API Gateway
```

Stage

```
prod
```

API Gateway sẽ gọi Lambda mỗi khi người dùng gửi yêu cầu từ Website.

---

## Bước 6: Chuẩn bị AI Workflow

Lambda cũng sẽ được AWS Step Functions gọi trong các chương tiếp theo.

Các nhiệm vụ bao gồm:

+ Tiền xử lý hình ảnh.
+ Kiểm tra định dạng.
+ Chuẩn bị dữ liệu cho Amazon Rekognition.
+ Lưu kết quả vào Amazon DynamoDB.

---

## Bước 7: Kiểm tra Lambda

Tại Lambda Console chọn:

```
Test
```

Tạo một Test Event.

Ví dụ:

```json
{
  "message": "Hello Smart Campus Guardian"
}
```

Nếu thành công, Lambda sẽ trả về:

```
StatusCode: 200
```

Đồng thời Logs sẽ xuất hiện trên Amazon CloudWatch.

---

## Best Practices

Để đảm bảo khả năng mở rộng và bảo mật, nên áp dụng các khuyến nghị sau:

✔ Không lưu trạng thái (Stateless).

✔ Sử dụng Environment Variables.

✔ Không Hard-code Access Key.

✔ Sử dụng IAM Role.

✔ Ghi Logs lên Amazon CloudWatch.

✔ Thiết lập Timeout phù hợp.

✔ Tái sử dụng Lambda Function khi có thể.

---

## Kiểm tra

Sau khi hoàn thành chương này, bạn sẽ có:

+ AWS Lambda Function.

+ IAM Execution Role.

+ Environment Variables.

+ API Gateway Trigger.

+ CloudWatch Logs.

+ Lambda sẵn sàng cho AI Workflow.

---

## Kết quả

Trong chương này, bạn đã triển khai thành công AWS Lambda cho hệ thống **Smart Campus Guardian**.

AWS Lambda đóng vai trò là tầng xử lý nghiệp vụ chính, tiếp nhận yêu cầu từ Amazon API Gateway và phối hợp với các dịch vụ AI trong toàn bộ hệ thống.

Trong chương tiếp theo, chúng ta sẽ triển khai **Amazon EventBridge** để tự động kích hoạt AI Workflow khi Camera tải hình ảnh lên Amazon S3.