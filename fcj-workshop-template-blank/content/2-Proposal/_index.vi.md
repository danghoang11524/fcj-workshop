---
title: "Bản đề xuất"
date: 2025-07-15
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

{{% notice info %}}
📌 **Đây là bản đề xuất của dự án Smart Campus Guardian – AI Campus Incident Detection Platform được triển khai trên Amazon Web Services (AWS).**
{{% /notice %}}

# Smart Campus Guardian

## AI Campus Incident Detection Platform

Smart Campus Guardian là hệ thống giám sát an toàn khuôn viên trường học sử dụng trí tuệ nhân tạo (AI) kết hợp kiến trúc Cloud Native trên Amazon Web Services (AWS). Hệ thống tự động phát hiện các sự cố như cháy, khói, đám đông bất thường hoặc các tình huống mất an toàn từ hình ảnh camera, sau đó phân tích, lưu trữ và gửi cảnh báo theo thời gian thực.

---

# 1. Tóm tắt tổng quan

Hiện nay, phần lớn các trường học vẫn giám sát an ninh bằng cách theo dõi camera thủ công, dẫn đến việc phản ứng chậm khi xảy ra sự cố.

Dự án Smart Campus Guardian ứng dụng các dịch vụ AI và Serverless của AWS để tự động hóa toàn bộ quy trình phát hiện và xử lý sự cố.

Hệ thống hoạt động theo mô hình **Event-Driven Architecture**, giúp tự động xử lý dữ liệu ngay khi có hình ảnh mới được tải lên từ camera.

---

# 2. Vấn đề cần giải quyết

## Vấn đề là gì?

Một số hạn chế của hệ thống giám sát truyền thống:

- Phụ thuộc hoàn toàn vào nhân viên giám sát.
- Khó phát hiện sự cố trong thời gian thực.
- Không có hệ thống đánh giá mức độ nguy hiểm.
- Không tự động gửi cảnh báo.
- Khó mở rộng khi số lượng camera tăng lên.

---

## Giải pháp

Smart Campus Guardian giải quyết bài toán bằng cách:

- Camera gửi hình ảnh lên Amazon S3.
- Amazon EventBridge tự động phát hiện sự kiện.
- AWS Step Functions điều phối quy trình AI.
- Amazon Rekognition nhận diện đối tượng trong ảnh.
- Amazon Bedrock phân tích mức độ rủi ro và tạo báo cáo.
- AWS Lambda xử lý nghiệp vụ.
- Amazon DynamoDB lưu trữ dữ liệu.
- Amazon SNS và Amazon SES gửi cảnh báo.
- Dashboard hiển thị kết quả theo thời gian thực.

---

# Lợi ích và hiệu quả đầu tư (ROI)

Việc triển khai Smart Campus Guardian mang lại nhiều lợi ích:

- Giảm thời gian phát hiện sự cố từ vài phút xuống chỉ còn vài giây.
- Giảm chi phí nhân sự giám sát.
- Tăng độ chính xác nhờ AI.
- Kiến trúc Serverless giúp tối ưu chi phí vận hành.
- Hệ thống dễ dàng mở rộng khi số lượng camera tăng.

---

# 3. Kiến trúc giải pháp

Kiến trúc được xây dựng theo mô hình **Cloud Native**, **Serverless** và **Event-Driven**.

Luồng hoạt động:

```
Camera
    │
    ▼
Amazon S3
    │
Object Created Event
    ▼
Amazon EventBridge
    │
    ▼
AWS Step Functions
    │
    ▼
AWS Lambda
    │
 ┌──┴──────────────┐
 ▼                 ▼
Amazon        Amazon
Rekognition   Bedrock
     │             │
     └──────┬──────┘
            ▼
     Amazon DynamoDB
            │
     ┌──────┴──────┐
     ▼             ▼
Amazon SNS    Amazon SES
            │
            ▼
      React Dashboard
```

![Sơ đồ kiến trúc](/images/2-Proposal/smart-campus-architecture.png)

---

# Các dịch vụ AWS sử dụng

- **Amazon S3**: Lưu trữ hình ảnh từ Camera.
- **Amazon EventBridge**: Điều phối sự kiện khi có ảnh mới.
- **AWS Step Functions**: Điều phối AI Workflow.
- **AWS Lambda**: Xử lý nghiệp vụ Serverless.
- **Amazon Rekognition**: Phát hiện đối tượng trong hình ảnh.
- **Amazon Bedrock**: Phân tích mức độ nguy hiểm bằng Generative AI.
- **Amazon DynamoDB**: Lưu trữ dữ liệu sự cố.
- **Amazon Cognito**: Xác thực người dùng.
- **Amazon API Gateway**: Cung cấp REST API.
- **Amazon SNS**: Gửi cảnh báo thời gian thực.
- **Amazon SES**: Gửi Email thông báo.
- **Amazon CloudWatch**: Giám sát và ghi log hệ thống.
- **Amazon CloudFront**: Phân phối Website.
- **IAM**: Quản lý quyền truy cập.

---

# Thiết kế các thành phần

### Frontend

- ReactJS
- Amazon S3
- Amazon CloudFront
- Amazon Cognito

### Backend

- Amazon API Gateway
- AWS Lambda

### AI Layer

- Amazon Rekognition
- Amazon Bedrock

### Data Layer

- Amazon DynamoDB

### Monitoring

- Amazon CloudWatch

### Notification

- Amazon SNS
- Amazon SES

---

# 4. Triển khai kỹ thuật

## Các giai đoạn triển khai

### Giai đoạn 1

Chuẩn bị môi trường AWS.

### Giai đoạn 2

Triển khai IAM và Amazon S3.

### Giai đoạn 3

Triển khai Amazon Cognito và API Gateway.

### Giai đoạn 4

Triển khai AWS Lambda.

### Giai đoạn 5

Xây dựng AI Workflow bằng EventBridge và Step Functions.

### Giai đoạn 6

Tích hợp Amazon Rekognition và Amazon Bedrock.

### Giai đoạn 7

Lưu dữ liệu vào Amazon DynamoDB.

### Giai đoạn 8

Triển khai SNS, SES và CloudWatch.

### Giai đoạn 9

Triển khai Dashboard.

### Giai đoạn 10

Kiểm thử toàn bộ hệ thống.

---

## Yêu cầu kỹ thuật

- AWS Account.
- AWS IAM User.
- AWS Region (ap-southeast-1).
- AWS CLI.
- Visual Studio Code.
- Node.js.
- Python 3.12.
- ReactJS.
- Git.

---

# 5. Thời gian & Mốc quan trọng

| Tuần | Công việc |
|------|-----------|
| Tuần 1 | Thiết kế kiến trúc |
| Tuần 2 | Triển khai hạ tầng AWS |
| Tuần 3 | Phát triển Backend |
| Tuần 4 | Xây dựng AI Workflow |
| Tuần 5 | Dashboard và Authentication |
| Tuần 6 | Kiểm thử và hoàn thiện |

---

# 6. Dự toán chi phí

Có thể sử dụng AWS Free Tier trong quá trình học tập và thử nghiệm.

Tham khảo:

https://calculator.aws

## Chi phí hạ tầng (ước tính)

| Dịch vụ | Chi phí/tháng |
|----------|---------------|
| Amazon S3 | ~2 USD |
| Lambda | Free Tier |
| DynamoDB | Free Tier |
| API Gateway | Free Tier |
| EventBridge | Free Tier |
| Step Functions | Free Tier |
| CloudWatch | ~2 USD |
| SNS | Free Tier |
| SES | <1 USD |
| Rekognition | Theo số lượng ảnh |
| Bedrock | Theo số lượng Token |

**Tổng chi phí thử nghiệm:** khoảng **5–10 USD/tháng**.

---

# 7. Đánh giá rủi ro

## Ma trận rủi ro

| Rủi ro | Mức độ |
|----------|---------|
| AI nhận diện sai | Trung bình |
| Camera mất kết nối | Cao |
| Chi phí AI tăng | Trung bình |
| Quá tải API | Thấp |
| Lỗi Workflow | Thấp |

---

## Chiến lược giảm thiểu

- Sử dụng CloudWatch Alarm.
- Thiết lập Retry trong Step Functions.
- Áp dụng IAM Least Privilege.
- Giới hạn quyền truy cập API bằng Amazon Cognito.
- Sử dụng Billing Alarm để theo dõi chi phí.

---

## Phương án dự phòng

- Retry Workflow.
- Lưu log trên CloudWatch.
- Backup dữ liệu.
- Tự động gửi cảnh báo khi Workflow thất bại.

---

# 8. Kết quả kỳ vọng

## Cải tiến kỹ thuật

- Xây dựng hệ thống Cloud Native hoàn chỉnh.
- Triển khai kiến trúc Event-Driven.
- Tích hợp AI Vision và Generative AI.
- Giảm thời gian xử lý sự cố xuống chỉ còn vài giây.
- Tự động hóa toàn bộ quy trình giám sát.

---

## Giá trị lâu dài

Smart Campus Guardian có thể mở rộng để ứng dụng trong:

- Trường học thông minh (Smart Campus).
- Nhà máy thông minh (Smart Factory).
- Bệnh viện.
- Trung tâm thương mại.
- Tòa nhà văn phòng.
- Khu công nghiệp.
- Thành phố thông minh (Smart City).

Đây là một giải pháp Cloud Native hiện đại, tận dụng sức mạnh của AWS AI Services và Serverless Computing để xây dựng hệ thống giám sát thông minh, dễ mở rộng, tối ưu chi phí và đáp ứng các tiêu chuẩn kiến trúc của AWS.