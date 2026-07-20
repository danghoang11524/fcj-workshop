---
title : "Kiến trúc hệ thống"
date : 2025-07-14
weight : 2
chapter : true
pre : " <b> 5.2. </b> "
---

# Kiến trúc hệ thống

## Giới thiệu

Trong chương này, chúng ta sẽ tìm hiểu kiến trúc tổng thể của dự án **Smart Campus Guardian – AI Campus Incident Detection Platform**.

Hệ thống được xây dựng theo mô hình **Cloud Native**, kết hợp giữa **Serverless Architecture** và **Event-Driven Architecture** trên nền tảng Amazon Web Services (AWS). Kiến trúc này giúp hệ thống có khả năng mở rộng linh hoạt, giảm chi phí vận hành và tự động xử lý các sự kiện phát sinh từ Camera AI theo thời gian thực.

Toàn bộ quy trình từ khi Camera tải hình ảnh lên Amazon S3 đến khi gửi cảnh báo cho quản trị viên đều được xử lý hoàn toàn tự động thông qua các dịch vụ được quản lý (Managed Services) của AWS.

---

# Kiến trúc tổng thể

Kiến trúc của hệ thống được chia thành các tầng chính sau:

- Frontend Layer
- Authentication Layer
- API Layer
- Event Processing Layer
- AI Analysis Layer
- Data Layer
- Notification Layer
- Monitoring Layer

Mỗi tầng đảm nhận một nhiệm vụ riêng biệt nhằm đảm bảo hệ thống dễ mở rộng, dễ bảo trì và đáp ứng kiến trúc Well-Architected Framework của AWS.

---

## Kiến trúc triển khai

Hình dưới đây mô tả toàn bộ kiến trúc của hệ thống Smart Campus Guardian trên AWS.

![System Architecture](/images/5-Workshop/5.2/5.2.1/system-architecture.png)

### Mô tả kiến trúc

#### Frontend Layer

Giao diện người dùng được phát triển bằng **ReactJS** và triển khai trên **Amazon S3 Static Website Hosting**.

Người dùng truy cập hệ thống thông qua **Amazon CloudFront**, giúp:

- Tăng tốc độ truy cập toàn cầu.
- Hỗ trợ HTTPS.
- Giảm độ trễ.
- Phân phối nội dung hiệu quả.

---

#### Authentication Layer

Hệ thống sử dụng **Amazon Cognito** để quản lý người dùng.

Cognito chịu trách nhiệm:

- Đăng ký tài khoản.
- Đăng nhập.
- Quản lý User Pool.
- Cấp JWT Access Token.

Sau khi xác thực thành công, người dùng sẽ sử dụng Access Token để gọi các API thông qua API Gateway.

---

#### API Layer

Các yêu cầu từ Dashboard sẽ được gửi đến **Amazon API Gateway**.

API Gateway sẽ:

- Xác thực người dùng.
- Định tuyến yêu cầu.
- Gọi AWS Lambda Backend.
- Ghi log hoạt động.

Lambda Backend sẽ thực hiện các nghiệp vụ và truy xuất dữ liệu từ Amazon DynamoDB.

---

#### Event Processing Layer

Khi Camera tải hình ảnh lên **Amazon S3 Image Bucket**, hệ thống sẽ tự động kích hoạt chuỗi xử lý sự kiện.

Amazon EventBridge sẽ phát hiện sự kiện Object Created và kích hoạt **AWS Step Functions** để điều phối toàn bộ quy trình AI.

Nhờ mô hình Event-Driven, hệ thống có thể xử lý đồng thời nhiều Camera mà không cần can thiệp thủ công.

---

#### AI Analysis Layer

Đây là thành phần quan trọng nhất của hệ thống.

Sau khi Step Functions được kích hoạt:

- AWS Lambda tiền xử lý hình ảnh.
- Amazon Rekognition nhận diện đối tượng trong ảnh.
- Amazon Bedrock phân tích kết quả nhận diện bằng AI Generative.

Hệ thống có thể phát hiện các tình huống như:

- Cháy (Fire)
- Khói (Smoke)
- Đông người (Crowd)
- Người khả nghi
- Phương tiện

Amazon Bedrock tiếp tục đánh giá mức độ nguy hiểm, sinh báo cáo AI và đề xuất phương án xử lý.

---

#### Data Layer

Hệ thống sử dụng **Amazon DynamoDB** để lưu trữ dữ liệu.

Bao gồm:

- Incident
- Camera Information
- AI Report
- Alert History

Trong khi đó Amazon S3 lưu trữ:

- Hình ảnh gốc.
- Báo cáo.
- Dữ liệu AI Output.

---

#### Notification Layer

Khi AI xác định sự cố có mức độ **HIGH** hoặc **CRITICAL**, hệ thống sẽ tự động gửi cảnh báo.

Amazon SNS dùng để gửi thông báo tức thời.

Amazon SES dùng để gửi Email cho quản trị viên.

---

#### Monitoring Layer

Toàn bộ hệ thống được giám sát bằng **Amazon CloudWatch**.

CloudWatch thu thập:

- Logs
- Metrics
- Dashboard
- Alarm

Điều này giúp quản trị viên dễ dàng theo dõi hoạt động của hệ thống và xử lý sự cố.

---

# AI Workflow

Ngoài kiến trúc tổng thể, hệ thống còn sử dụng quy trình AI tự động để xử lý hình ảnh từ Camera.

Quy trình này được điều phối bởi Amazon EventBridge và AWS Step Functions, sau đó kết hợp Amazon Rekognition và Amazon Bedrock để phân tích nội dung hình ảnh và đưa ra quyết định.

![AI Workflow](/images/5-Workshop/5.2/5.2.2/ai-workflow.png)

---

## Luồng xử lý AI

Quy trình xử lý của hệ thống gồm các bước sau:

1. Camera tải hình ảnh lên Amazon S3.
2. Amazon S3 phát sinh sự kiện Object Created.
3. Amazon EventBridge phát hiện sự kiện.
4. AWS Step Functions khởi động quy trình xử lý.
5. AWS Lambda tiền xử lý hình ảnh.
6. Amazon Rekognition nhận diện đối tượng.
7. Amazon Bedrock đánh giá mức độ nguy hiểm và sinh báo cáo AI.
8. AWS Lambda lưu kết quả vào Amazon DynamoDB.
9. Amazon SNS gửi cảnh báo tức thời.
10. Amazon SES gửi Email cho quản trị viên.
11. Dashboard hiển thị thông tin sự cố thông qua API Gateway.
12. Amazon CloudWatch ghi nhận Logs và Metrics của toàn bộ hệ thống.

---

## Kết quả

Sau khi hoàn thành chương này, bạn đã có cái nhìn tổng quan về kiến trúc của dự án Smart Campus Guardian và hiểu được cách các dịch vụ AWS phối hợp với nhau trong toàn bộ hệ thống.

Ở các chương tiếp theo, chúng ta sẽ lần lượt triển khai từng thành phần của kiến trúc, bắt đầu từ việc chuẩn bị môi trường, cấu hình IAM, Amazon S3 và các dịch vụ AI trên AWS.