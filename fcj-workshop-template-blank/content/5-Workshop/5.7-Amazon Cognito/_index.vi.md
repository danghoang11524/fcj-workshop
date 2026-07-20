---
title : "Triển khai Amazon Cognito"
date : 2025-07-14
weight : 7
chapter : true
pre : " <b> 5.7. </b> "
---

# Triển khai Amazon Cognito

## Giới thiệu

Amazon Cognito là dịch vụ quản lý danh tính (Identity Management) của AWS, giúp xây dựng hệ thống xác thực người dùng một cách an toàn mà không cần tự phát triển cơ chế Authentication.

Trong dự án **Smart Campus Guardian – AI Campus Incident Detection Platform**, Amazon Cognito được sử dụng để quản lý tài khoản quản trị viên, xác thực đăng nhập và cấp **JWT Token** cho Website React. Token này sẽ được sử dụng để xác thực khi gọi các API thông qua Amazon API Gateway.

---

## Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ:

+ Tạo Amazon Cognito User Pool.
+ Tạo App Client.
+ Tạo tài khoản quản trị viên.
+ Cấu hình Password Policy.
+ Đăng nhập và nhận JWT Token.
+ Chuẩn bị Authentication cho Amazon API Gateway.

---

# Kiến trúc Amazon Cognito

Amazon Cognito chịu trách nhiệm xác thực người dùng trước khi truy cập vào các API của hệ thống Smart Campus Guardian.

Sau khi đăng nhập thành công, Cognito sẽ cấp JWT Token để Amazon API Gateway xác minh quyền truy cập trước khi chuyển yêu cầu đến AWS Lambda.

![Cognito Architecture](/images/5-Workshop/5.7/5.7.1/cognito-architecture.png)

---

## Giải thích kiến trúc

Quy trình xác thực hoạt động như sau:

1. Người dùng truy cập Website React thông qua Amazon CloudFront.
2. Website hiển thị màn hình đăng nhập.
3. Amazon Cognito xác thực thông tin tài khoản.
4. Sau khi đăng nhập thành công, Cognito cấp:
   - Access Token
   - ID Token
   - Refresh Token
5. Website React sử dụng Access Token để gửi yêu cầu đến Amazon API Gateway.
6. API Gateway xác minh JWT Token trước khi cho phép truy cập các API phía sau.

Việc sử dụng Amazon Cognito giúp hệ thống tăng cường bảo mật, giảm công sức phát triển và dễ dàng mở rộng trong tương lai.

---

# Các bước triển khai

## Bước 1: Mở Amazon Cognito

Đăng nhập **AWS Management Console**.

Tìm kiếm:

```
Amazon Cognito
```

Chọn:

```
Create User Pool
```

![Create User Pool](/images/5-Workshop/5.7/5.7.2/create-userpool.png)

---

## Bước 2: Cấu hình phương thức đăng nhập

Tại mục **Sign-in Options**, chọn:

```
Email
```

Authentication Method

```
Username + Password
```

Điều này cho phép người dùng đăng nhập bằng địa chỉ Email.

---

## Bước 3: Cấu hình Password Policy

Thiết lập Password Policy như sau:

Minimum Length

```
8
```

Yêu cầu:

✔ Uppercase Letter

✔ Lowercase Letter

✔ Number

✔ Special Character

Password Policy mạnh sẽ giúp tăng cường bảo mật cho hệ thống.

---

## Bước 4: Tạo User Pool

User Pool Name

```
SmartCampusGuardianUserPool
```

Đây là nơi lưu trữ toàn bộ tài khoản người dùng của hệ thống.

---

## Bước 5: Tạo App Client

Chọn:

```
Create App Client
```

Tên App Client

```
SmartCampusGuardianWeb
```

Authentication Flow

```
ALLOW_USER_PASSWORD_AUTH
```

```
ALLOW_REFRESH_TOKEN_AUTH
```

Callback URL

```
https://xxxxxxxx.cloudfront.net
```

Trong Workshop, có thể sử dụng Domain CloudFront được tạo ở chương trước.

---

## Bước 6: Tạo tài khoản quản trị

Chọn:

```
Create User
```

Thông tin:

Email

```
admin@campus.local
```

Password

```
Admin@123456
```

Người dùng này sẽ được sử dụng để đăng nhập vào Dashboard quản trị.

---

## Bước 7: Kiểm tra đăng nhập

Đăng nhập bằng tài khoản vừa tạo.

Sau khi đăng nhập thành công, Amazon Cognito sẽ trả về:

+ Access Token

+ ID Token

+ Refresh Token

Website React sẽ sử dụng **Access Token** để gọi Amazon API Gateway trong các chương tiếp theo.

---

## Best Practices

Để đảm bảo hệ thống an toàn, nên áp dụng các khuyến nghị sau:

✔ Bật Email Verification.

✔ Sử dụng Password Policy mạnh.

✔ Kích hoạt Multi-Factor Authentication (MFA) nếu triển khai Production.

✔ Không lưu JWT Token trong Local Storage nếu xử lý dữ liệu nhạy cảm.

✔ Không tự xây dựng hệ thống Authentication khi đã có Amazon Cognito.

---

## Kiểm tra

Sau khi hoàn thành chương này, bạn sẽ có:

+ Amazon Cognito User Pool.

+ App Client.

+ Tài khoản quản trị viên.

+ JWT Access Token.

+ Authentication sẵn sàng tích hợp với Amazon API Gateway.

---

## Kết quả

Trong chương này, bạn đã triển khai thành công Amazon Cognito để quản lý người dùng và xác thực truy cập cho hệ thống **Smart Campus Guardian**.

Amazon Cognito giúp cấp phát JWT Token an toàn, hỗ trợ quản lý người dùng và tích hợp trực tiếp với Amazon API Gateway mà không cần xây dựng hệ thống xác thực riêng.

Trong chương tiếp theo, chúng ta sẽ triển khai **Amazon API Gateway** để xây dựng các API REST và kết nối với AWS Lambda.