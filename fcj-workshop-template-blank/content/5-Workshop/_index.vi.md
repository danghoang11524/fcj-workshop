---
title : "Workshop"
date : 2025-07-15
weight : 5
chapter : true
pre : "<b>5.</b>"
---
# Workshop

## Giới thiệu

Trong phần này, chúng ta sẽ triển khai hệ thống **Smart Campus Guardian – AI Campus Incident Detection Platform** trên nền tảng Amazon Web Services (AWS).

Đây là hệ thống giám sát an toàn khuôn viên trường học sử dụng kiến trúc **Cloud Native**, **Serverless**, kết hợp các dịch vụ **AI/ML** của AWS để tự động phát hiện, phân tích và cảnh báo các sự cố như cháy nổ, khói, đám đông bất thường hoặc các tình huống mất an toàn.

Workshop được xây dựng theo mô hình **Event-Driven Architecture**, trong đó toàn bộ quy trình từ lúc Camera tải hình ảnh lên Amazon S3 đến khi gửi cảnh báo đều được xử lý tự động thông qua các dịch vụ AWS.

---

## Mục tiêu

Sau khi hoàn thành Workshop, bạn sẽ có thể:

- Triển khai một kiến trúc Cloud Native hoàn chỉnh trên AWS.
- Xây dựng hệ thống xử lý sự kiện theo mô hình Event-Driven.
- Tích hợp AI Vision với Amazon Rekognition để phân tích hình ảnh.
- Sử dụng Amazon Bedrock để đánh giá mức độ rủi ro và sinh báo cáo AI.
- Xây dựng REST API bằng Amazon API Gateway và AWS Lambda.
- Lưu trữ dữ liệu bằng Amazon DynamoDB.
- Gửi cảnh báo thời gian thực bằng Amazon SNS và Amazon SES.
- Giám sát hệ thống bằng Amazon CloudWatch.
- Triển khai Frontend React trên Amazon S3 và CloudFront.

---

## Kiến trúc hệ thống

Hệ thống bao gồm các thành phần chính:

- Amazon S3 lưu trữ hình ảnh từ Camera.
- Amazon EventBridge tiếp nhận sự kiện khi có ảnh mới.
- AWS Step Functions điều phối toàn bộ AI Workflow.
- AWS Lambda xử lý nghiệp vụ.
- Amazon Rekognition nhận diện đối tượng trong ảnh.
- Amazon Bedrock đánh giá mức độ nguy hiểm và sinh báo cáo AI.
- Amazon DynamoDB lưu trữ thông tin sự cố.
- Amazon SNS và Amazon SES gửi cảnh báo.
- Amazon CloudWatch theo dõi và giám sát hệ thống.
- Amazon Cognito xác thực người dùng.
- Amazon API Gateway cung cấp API cho ứng dụng.
- Amazon CloudFront phân phối Website toàn cầu.

---

## Nội dung Workshop

Workshop gồm các chương sau:

**5.1.** Giới thiệu

**5.2.** Kiến trúc hệ thống

**5.3.** Chuẩn bị môi trường

**5.4.** Tạo IAM User và IAM Role

**5.5.** Triển khai Amazon S3

**5.6.** AI Workflow

**5.7.** Triển khai Amazon Cognito

**5.8.** Triển khai Amazon API Gateway

**5.9.** Triển khai AWS Lambda

**5.10.** Triển khai Amazon EventBridge

**5.11.** Triển khai AWS Step Functions

**5.12.** Triển khai Amazon Rekognition

**5.13.** Triển khai Amazon Bedrock

**5.14.** Triển khai Amazon DynamoDB

**5.15.** Triển khai Amazon SNS

**5.16.** Triển khai Amazon SES

**5.17.** Giám sát với Amazon CloudWatch

**5.18.** Dashboard và Frontend

**5.19.** Kiểm thử hệ thống

**5.20.** Dọn dẹp tài nguyên

**5.21.** Tổng kết

---

## Kết quả mong đợi

Sau khi hoàn thành Workshop, bạn sẽ triển khai thành công một hệ thống **AI Campus Incident Detection Platform** trên AWS với đầy đủ các thành phần từ lưu trữ, xử lý AI, quản lý người dùng, API, cơ sở dữ liệu, cảnh báo và giám sát.

Kiến trúc được thiết kế theo các nguyên tắc **AWS Well-Architected Framework**, đảm bảo tính bảo mật, khả năng mở rộng, tính sẵn sàng cao và tối ưu chi phí.

