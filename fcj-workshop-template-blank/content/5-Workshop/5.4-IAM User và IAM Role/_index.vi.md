---
title : "Tạo IAM User và IAM Role"
date : 2025-07-14
weight : 4
chapter : true
pre : " <b> 5.4. </b> "
---

# Tạo IAM User và IAM Role

## Giới thiệu

Trong chương này, chúng ta sẽ cấu hình **AWS Identity and Access Management (IAM)** để quản lý quyền truy cập cho người dùng và các dịch vụ trong hệ thống **Smart Campus Guardian – AI Campus Incident Detection Platform**.

IAM giúp kiểm soát việc xác thực và phân quyền cho từng tài nguyên AWS, đồng thời đảm bảo hệ thống tuân thủ nguyên tắc **Least Privilege**, chỉ cấp những quyền cần thiết cho từng thành phần.

---

## Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ:

- Tạo IAM User để quản trị Workshop.
- Tạo IAM Role cho AWS Lambda.
- Tạo IAM Role cho AWS Step Functions.
- Hiểu cách sử dụng IAM Policy.
- Áp dụng nguyên tắc Least Privilege.
- Không sử dụng Access Key trong mã nguồn.

---

# Kiến trúc IAM

Hệ thống sử dụng **IAM User** để quản trị AWS Console và **IAM Role** để các dịch vụ AWS truy cập tài nguyên một cách an toàn.

![IAM Architecture](/images/5-Workshop/5.4/iam-architecture.png)

---

## Giải thích kiến trúc

Trong Smart Campus Guardian, quyền truy cập được phân chia thành hai nhóm:

### IAM User

IAM User được sử dụng bởi quản trị viên để:

- Đăng nhập AWS Management Console.
- Quản lý tài nguyên AWS.
- Triển khai Workshop.

Trong môi trường Workshop, IAM User sẽ được cấp quyền **AdministratorAccess** để thuận tiện cho việc thực hành.

### IAM Role

IAM Role được sử dụng bởi các dịch vụ AWS thay vì Access Key.

Các Role sẽ được gán cho:

- AWS Lambda
- AWS Step Functions

Thông qua IAM Role, các dịch vụ có thể truy cập:

- Amazon S3
- Amazon DynamoDB
- Amazon Rekognition
- Amazon Bedrock
- Amazon SNS
- Amazon CloudWatch

Điều này giúp tăng cường bảo mật và tuân thủ nguyên tắc **Least Privilege**.

---

## Bước 1: Mở IAM Console

Đăng nhập **AWS Management Console**.

Tìm kiếm dịch vụ:

```
IAM
```

Chọn:

```
Identity and Access Management (IAM)
```

---

## Bước 2: Tạo IAM User

Trong IAM Console:

```
Users
        ↓
Create User
```

Đặt tên:

```
campus-admin
```

Đánh dấu:

```
Provide user access to the AWS Management Console
```

Chọn:

```
I want to create an IAM User
```

![Create User](/images/5-Workshop/5.4/5.4.1-IAM/create-user.png)

---

## Bước 3: Gán quyền cho IAM User

Chọn:

```
Attach policies directly
```

Thêm Policy:

```
AdministratorAccess
```

{{% notice warning %}}
Workshop sử dụng **AdministratorAccess** để đơn giản hóa quá trình triển khai.

Trong môi trường Production, nên tạo IAM Policy riêng và chỉ cấp các quyền thực sự cần thiết theo nguyên tắc **Least Privilege**.
{{% /notice %}}

---

## Bước 4: Tạo IAM Role cho AWS Lambda

Trong IAM Console:

```
Roles
        ↓
Create Role
```

Trusted Entity:

```
AWS Service
```

Service:

```
Lambda
```

Tên Role:

```
SmartCampusLambdaRole
```

![Create IAM Role](/images/5-Workshop/5.4/5.4.2/create-role.png)

---

## Bước 5: Gán Policy cho Lambda Role

Thêm các Policy sau:

- AmazonS3FullAccess
- AmazonDynamoDBFullAccess
- AmazonSNSFullAccess
- AmazonRekognitionFullAccess
- AmazonBedrockFullAccess
- CloudWatchLogsFullAccess

Sau khi hoàn tất, chọn **Create Role**.

---

## Bước 6: Tạo IAM Role cho Step Functions

Tiếp tục tạo một IAM Role mới.

Tên Role:

```
SmartCampusStepFunctionsRole
```

Trusted Entity:

```
AWS Step Functions
```

Thêm các Policy:

- AWSLambdaRole
- AmazonEventBridgeFullAccess

Hoàn tất việc tạo Role.

---

## Best Practices

Để đảm bảo an toàn cho hệ thống, hãy tuân thủ các nguyên tắc sau:

- Không Hard-code Access Key trong Source Code.
- Ưu tiên sử dụng IAM Role thay cho Access Key.
- Áp dụng nguyên tắc Least Privilege.
- Bật MFA cho IAM User.
- Thường xuyên rà soát và loại bỏ các Access Key không còn sử dụng.

---

## Kết quả

Sau khi hoàn thành chương này, bạn đã:

- Tạo thành công IAM User **campus-admin**.
- Tạo IAM Role cho AWS Lambda.
- Tạo IAM Role cho AWS Step Functions.
- Cấu hình quyền truy cập theo nguyên tắc Least Privilege.
- Chuẩn bị đầy đủ quyền để triển khai các dịch vụ AWS trong những chương tiếp theo.

Trong chương tiếp theo, chúng ta sẽ tạo **Amazon S3 Bucket** để lưu trữ hình ảnh từ Camera AI và cấu hình sự kiện kích hoạt quy trình xử lý của hệ thống.