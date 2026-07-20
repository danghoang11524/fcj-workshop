---
title : "Chuẩn bị môi trường"
date : 2025-07-14
weight : 3
chapter : true
pre : " <b> 5.3. </b> "
---

# Chuẩn bị môi trường

## Giới thiệu

Trước khi bắt đầu triển khai **Smart Campus Guardian – AI Campus Incident Detection Platform**, chúng ta cần chuẩn bị đầy đủ môi trường phát triển và tài khoản AWS.

Chương này sẽ hướng dẫn các yêu cầu cần thiết để đảm bảo quá trình triển khai diễn ra thuận lợi trong các chương tiếp theo.

---

# Môi trường triển khai

Hình dưới đây mô tả các thành phần cần chuẩn bị trước khi triển khai Workshop.

![Workshop Prerequisites](/images/5-Workshop/5.3/prerequisite.png)

---

## Điều kiện

Trước khi triển khai, hãy đảm bảo bạn đã chuẩn bị:

- AWS Account
- IAM Administrator User
- AWS CLI
- Visual Studio Code
- Git
- Node.js 20 hoặc mới hơn
- Java 21
- Kết nối Internet ổn định

Để đảm bảo tính nhất quán trong toàn bộ Workshop, chúng ta sẽ sử dụng Region:

```text
ap-southeast-1 (Singapore)
```

---

## Kiểm tra AWS CLI

Sau khi cài đặt AWS CLI, hãy kiểm tra phiên bản bằng lệnh:

```bash
aws --version
```

Ví dụ kết quả:

```text
aws-cli/2.x.x Python/3.x Windows/64-bit
```

---

## Kiểm tra thông tin tài khoản AWS

Sau khi cấu hình AWS CLI, chạy lệnh sau để xác nhận tài khoản đang sử dụng:

```bash
aws sts get-caller-identity
```

Kết quả mong đợi:

```json
{
  "UserId": "...",
  "Account": "123456789012",
  "Arn": "arn:aws:iam::123456789012:user/WorkshopAdmin"
}
```

Nếu lệnh thực thi thành công, AWS CLI đã được cấu hình chính xác.

---

## Quyền IAM yêu cầu

Tài khoản sử dụng trong Workshop cần có quyền quản lý các dịch vụ sau:

- IAM
- Amazon S3
- Amazon CloudFront
- Amazon Cognito
- Amazon API Gateway
- AWS Lambda
- Amazon EventBridge
- AWS Step Functions
- Amazon Rekognition
- Amazon Bedrock
- Amazon DynamoDB
- Amazon SNS
- Amazon SES
- Amazon CloudWatch

{{% notice warning %}}
Khuyến nghị sử dụng **IAM User** hoặc **IAM Role** có quyền Administrator trong môi trường học tập. Không nên sử dụng tài khoản Root để triển khai Workshop.
{{% /notice %}}

---

## Các công cụ sử dụng

Trong Workshop này chúng ta sẽ sử dụng:

| Công cụ | Mục đích |
|----------|----------|
| AWS Management Console | Quản lý tài nguyên AWS |
| AWS CLI | Quản lý tài nguyên bằng dòng lệnh |
| Visual Studio Code | Phát triển mã nguồn |
| Git | Quản lý mã nguồn |
| Node.js | Chạy Frontend React |
| Java 21 | Phát triển Backend |

---

## Nội dung Workshop

Sau khi hoàn tất phần chuẩn bị, chúng ta sẽ lần lượt triển khai các thành phần của hệ thống theo đúng kiến trúc đã thiết kế.

Các chương tiếp theo bao gồm:

1. IAM
2. Amazon S3
3. Amazon Cognito
4. Amazon API Gateway
5. AWS Lambda
6. Amazon EventBridge
7. AWS Step Functions
8. Amazon Rekognition
9. Amazon Bedrock
10. Amazon DynamoDB
11. Amazon SNS
12. Amazon SES
13. Amazon CloudWatch
14. Dashboard
15. Testing
16. Cleanup

---

## Kết quả

Sau khi hoàn thành chương này, bạn đã chuẩn bị đầy đủ môi trường phát triển, tài khoản AWS và các công cụ cần thiết để bắt đầu triển khai hệ thống **Smart Campus Guardian**.

Trong chương tiếp theo, chúng ta sẽ bắt đầu cấu hình **IAM** và tạo các quyền truy cập cần thiết cho toàn bộ hệ thống.