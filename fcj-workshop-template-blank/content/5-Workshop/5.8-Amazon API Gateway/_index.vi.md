---
title : "Triển khai Amazon API Gateway"
date : 2025-07-14
weight : 8
chapter : true
pre : " <b> 5.8. </b> "
---

# Triển khai Amazon API Gateway

## Giới thiệu

Amazon API Gateway là dịch vụ giúp xây dựng, quản lý và bảo vệ các REST API trên AWS.

Trong dự án **Smart Campus Guardian – AI Campus Incident Detection Platform**, Amazon API Gateway đóng vai trò là cổng giao tiếp giữa Website React và các dịch vụ Backend chạy trên AWS Lambda.

Ngoài chức năng định tuyến (Routing), API Gateway còn tích hợp trực tiếp với Amazon Cognito để xác thực JWT Token trước khi cho phép truy cập vào các API của hệ thống.

---

## Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ:

+ Tạo REST API.
+ Tạo các Resource và Method.
+ Kết nối API Gateway với AWS Lambda.
+ Cấu hình JWT Authorization bằng Amazon Cognito.
+ Deploy API lên Stage Production.
+ Bật CloudWatch Logging.

---

# Kiến trúc Amazon API Gateway

Amazon API Gateway đóng vai trò là lớp API của hệ thống, tiếp nhận toàn bộ yêu cầu từ Website React và chuyển tiếp đến AWS Lambda sau khi xác thực JWT Token.

![API Gateway Architecture](/images/5-Workshop/5.8/5.8.1/api-architecture.png)

---

## Giải thích kiến trúc

Quy trình xử lý yêu cầu như sau:

1. Người dùng đăng nhập vào Website React.
2. Amazon Cognito cấp JWT Access Token.
3. Website gửi Access Token trong Header của mỗi API Request.
4. Amazon API Gateway xác minh JWT Token.
5. Nếu hợp lệ, API Gateway chuyển yêu cầu đến AWS Lambda.
6. Lambda xử lý nghiệp vụ và trả kết quả về Website.
7. CloudWatch ghi lại Logs và Metrics của API.

Việc sử dụng API Gateway giúp hệ thống tăng cường bảo mật, dễ dàng mở rộng và quản lý toàn bộ API tập trung.

---

# Danh sách API

| Method | Endpoint | Chức năng |
|---------|----------|-----------|
| GET | /dashboard | Lấy dữ liệu Dashboard |
| GET | /incidents | Danh sách sự cố |
| GET | /incidents/{id} | Chi tiết sự cố |
| POST | /camera/upload | Upload hình ảnh Camera |
| GET | /cameras | Danh sách Camera |
| POST | /login | Đăng nhập (Amazon Cognito) |

---

# Các bước triển khai

## Bước 1: Mở Amazon API Gateway

Đăng nhập **AWS Management Console**.

Tìm kiếm:

```
Amazon API Gateway
```

Chọn:

```
Create API
```

Sau đó chọn:

```
REST API
```

---

## Bước 2: Tạo REST API

API Name

```
SmartCampusGuardianAPI
```

Endpoint Type

```
Regional
```

Description

```
REST API for Smart Campus Guardian
```

![Create API](/images/5-Workshop/5.8/5.8.2/create-api.png)

---

## Bước 3: Tạo Resource

Tạo các Resource sau:

```
/dashboard

/incidents

/cameras

/camera/upload
```

Các Resource này sẽ đại diện cho các nhóm API của hệ thống.

---

## Bước 4: Tạo Method

Tạo các HTTP Method phù hợp:

```
GET

POST
```

Sau đó cấu hình Integration:

```
Integration Type

↓

Lambda Function
```

Chọn Lambda Function:

```
SmartCampusHandler
```

---

## Bước 5: Cấu hình Authorization

Authorization Type

```
Amazon Cognito
```

User Pool

```
SmartCampusGuardianUserPool
```

Identity Source

```
Authorization Header
```

API Gateway sẽ xác thực JWT Token trước khi chuyển yêu cầu đến Lambda.

---

## Bước 6: Deploy API

Chọn:

```
Deploy API
```

Stage Name

```
prod
```

Sau khi Deploy, API Gateway sẽ cung cấp Invoke URL.

Ví dụ:

```
https://abc123.execute-api.ap-southeast-1.amazonaws.com/prod
```

---

## Bước 7: Bật CloudWatch Logging

Trong Stage:

```
Logs / Tracing
```

Bật:

```
Enable CloudWatch Logs

Enable Access Logging
```

CloudWatch sẽ ghi lại toàn bộ Request, Response và lỗi của API.

---

## Bước 8: Kiểm tra API

Sử dụng Postman hoặc Website React để gửi yêu cầu:

```
GET /dashboard
```

Header

```
Authorization

Bearer <Access Token>
```

Nếu Access Token hợp lệ, API sẽ trả về dữ liệu Dashboard.

---

## Best Practices

Để đảm bảo hiệu năng và bảo mật, nên áp dụng các khuyến nghị sau:

✔ Sử dụng JWT Authorization.

✔ Bật CloudWatch Logs.

✔ Bật Request Validation.

✔ Cấu hình CORS.

✔ Thiết lập Throttling.

✔ Sử dụng Stage riêng cho Development và Production.

---

## Kiểm tra

Sau khi hoàn thành chương này, bạn sẽ có:

+ REST API hoạt động.

+ Stage `prod`.

+ JWT Authorization bằng Amazon Cognito.

+ API Gateway kết nối với AWS Lambda.

+ CloudWatch Logging được bật.

---

## Kết quả

Trong chương này, bạn đã triển khai thành công Amazon API Gateway cho hệ thống **Smart Campus Guardian**.

API Gateway đóng vai trò là lớp trung gian giữa Website React và AWS Lambda, đồng thời chịu trách nhiệm xác thực JWT Token, quản lý API và ghi nhận Logs thông qua Amazon CloudWatch.

Trong chương tiếp theo, chúng ta sẽ triển khai **AWS Lambda** để xây dựng toàn bộ Backend Serverless xử lý các yêu cầu từ API Gateway.