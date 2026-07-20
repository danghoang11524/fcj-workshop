---
title : "Triển khai Amazon SES"
date : 2025-07-15
weight : 16
chapter : true
pre : " <b> 5.16. </b> "
---

# Triển khai Amazon SES

## Giới thiệu

Amazon Simple Email Service (Amazon SES) là dịch vụ gửi Email của AWS với khả năng mở rộng cao và độ tin cậy lớn.

Trong dự án **Smart Campus Guardian – AI Campus Incident Detection Platform**, Amazon SES được sử dụng để gửi Email cảnh báo chi tiết sau khi AI hoàn thành phân tích sự cố.

Sau khi AWS Step Functions điều phối AI Workflow, Lambda AI sẽ lưu Incident vào Amazon DynamoDB, gửi thông báo qua Amazon SNS và đồng thời sử dụng Amazon SES để gửi Email đến đội ngũ quản lý và bảo vệ.

---

## Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ:

+ Xác thực Email với Amazon SES.
+ Gửi Email bằng AWS Lambda.
+ Thiết lập Email Template.
+ Kiểm tra Email cảnh báo được gửi thành công.

---

# Kiến trúc Amazon SES

Amazon SES là dịch vụ gửi Email trong AI Workflow.

Sau khi Lambda AI hoàn thành xử lý Incident, hệ thống sẽ sử dụng Amazon SES để gửi Email cảnh báo đến người quản trị.

![SES Architecture](/images/5-Workshop/5.16/5.16.1/ses-architecture.png)

---

## Giải thích kiến trúc

Quy trình hoạt động như sau:

1. AWS Step Functions điều phối AI Workflow.
2. Lambda AI nhận kết quả từ Amazon Bedrock.
3. Lambda AI lưu Incident vào Amazon DynamoDB.
4. Lambda AI Publish thông báo đến Amazon SNS.
5. Lambda AI gọi Amazon SES để gửi Email.
6. Amazon CloudWatch ghi lại Logs và Metrics.

Việc tách riêng Amazon SES giúp hệ thống dễ dàng thay đổi mẫu Email hoặc mở rộng nhiều loại thông báo khác nhau mà không ảnh hưởng đến AI Workflow.

---

# Các bước triển khai

## Bước 1: Mở Amazon SES

Đăng nhập **AWS Management Console**.

Tìm kiếm:

```
Amazon SES
```

Chọn:

```
Verified Identities
```

↓

```
Create Identity
```

---

## Bước 2: Xác thực Email

Identity Type

```
Email Address
```

Email

```
security@school.edu
```

Nhấn:

```
Create Identity
```

Sau đó kiểm tra hộp thư và xác nhận Email.

![Verify Email](/images/5-Workshop/5.16/5.16.2/verify-email.png)

---

## Bước 3: Cập nhật Lambda AI

Sau khi Incident được lưu thành công, Lambda AI sẽ sử dụng API:

```
SendEmail
```

để gửi Email đến địa chỉ đã xác thực.

---

## Bước 4: Nội dung Email

Subject

```
Smart Campus Guardian - Incident Alert
```

Body

```text
Incident Detected

Location:
Building A

Risk Level:
HIGH

Summary:
Fire and smoke detected near Building A.

Recommended Action:
Notify campus security immediately and initiate evacuation.

Time:
2025-07-15 10:30 UTC
```

Trong môi trường Production, nên sử dụng Email Template để dễ quản lý và tái sử dụng.

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

Amazon SES

↓

Sent Emails
```

Kiểm tra Email đã được gửi đến địa chỉ:

```
security@school.edu
```

---

## Best Practices

✔ Xác thực Email trước khi gửi.

✔ Sử dụng Email Template.

✔ Không Hard-code địa chỉ Email trong mã nguồn.

✔ Chỉ gửi Email khi Risk Level là HIGH hoặc CRITICAL.

✔ Ghi Logs bằng Amazon CloudWatch.

---

## Kiểm tra

Sau khi hoàn thành chương này, bạn sẽ có:

+ Amazon SES Identity đã được xác thực.

+ Lambda AI gửi Email thành công.

+ Email cảnh báo tự động.

+ CloudWatch Logs ghi nhận quá trình gửi Email.

---

## Kết quả

Trong chương này, bạn đã triển khai thành công Amazon SES để gửi Email cảnh báo trong hệ thống **Smart Campus Guardian**.

Amazon SES giúp gửi thông tin chi tiết về sự cố như mức độ nguy hiểm, vị trí, thời gian và khuyến nghị xử lý đến đội ngũ quản lý, góp phần nâng cao khả năng phản ứng nhanh khi xảy ra tình huống khẩn cấp.

Trong chương tiếp theo, chúng ta sẽ triển khai **Amazon CloudWatch** để giám sát toàn bộ hệ thống và theo dõi Logs, Metrics cũng như Alarm của AI Workflow.