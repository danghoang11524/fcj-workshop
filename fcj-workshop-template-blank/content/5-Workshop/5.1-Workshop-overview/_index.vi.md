---
title : "Giới thiệu"
date : 2025-07-14
weight : 1
chapter : true
pre : " <b> 5.1. </b> "
---

# Giới thiệu về Smart Campus Guardian

Smart Campus Guardian là hệ thống giám sát sự cố thông minh dành cho khuôn viên trường học, được xây dựng hoàn toàn trên nền tảng AWS Cloud theo kiến trúc **Cloud Native** và **Event-Driven**.

Hệ thống sử dụng các dịch vụ AI của AWS để tự động phát hiện các sự cố như:

+ Cháy hoặc khói trong khuôn viên.
+ Người bị ngã hoặc bất động trong thời gian dài.
+ Tụ tập đông người bất thường.
+ Xâm nhập khu vực cấm.
+ Camera mất kết nối.

Sau khi phát hiện sự cố, hệ thống sẽ tự động phân tích mức độ nguy hiểm, lưu thông tin sự kiện, gửi cảnh báo đến bộ phận bảo vệ và hiển thị kết quả trên Dashboard theo thời gian thực.

---

#### Mục tiêu của Workshop

Trong workshop này, bạn sẽ triển khai một hệ thống giám sát thông minh sử dụng các dịch vụ AWS.

Sau khi hoàn thành workshop, bạn sẽ có thể:

+ Triển khai Website trên Amazon S3 và CloudFront.
+ Xây dựng API Serverless bằng Amazon API Gateway và AWS Lambda.
+ Lưu trữ hình ảnh từ Camera trên Amazon S3.
+ Xây dựng Workflow xử lý sự kiện bằng Amazon EventBridge và AWS Step Functions.
+ Sử dụng Amazon Rekognition để nhận diện đối tượng trong hình ảnh.
+ Sử dụng Amazon Bedrock để đánh giá mức độ rủi ro của sự cố.
+ Lưu dữ liệu Incident trên Amazon DynamoDB.
+ Gửi Email và SMS cảnh báo thông qua Amazon SES và Amazon SNS.
+ Giám sát toàn bộ hệ thống bằng Amazon CloudWatch.

---

#### Tổng quan kiến trúc hệ thống

Kiến trúc Smart Campus Guardian được chia thành các thành phần chính:

+ **Frontend Layer**  
Website được triển khai trên **Amazon S3 Static Website Hosting** và phân phối thông qua **Amazon CloudFront** nhằm tăng tốc độ truy cập và giảm độ trễ.

+ **Authentication Layer**  
Người dùng đăng nhập thông qua **Amazon Cognito**, sử dụng JWT Token để xác thực các API.

+ **Application Layer**  
Các yêu cầu từ người dùng được tiếp nhận bởi **Amazon API Gateway** và xử lý thông qua **AWS Lambda**.

+ **AI Processing Layer**  
Khi Camera tải hình ảnh lên Amazon S3, sự kiện sẽ kích hoạt **Amazon EventBridge** và **AWS Step Functions** để điều phối quy trình xử lý AI.

+ **AI Analysis Layer**  
Amazon Rekognition nhận diện đối tượng trong ảnh, sau đó Amazon Bedrock đánh giá mức độ nguy hiểm và đề xuất hành động phù hợp.

+ **Data Layer**  
Thông tin Incident được lưu trên **Amazon DynamoDB**, trong khi hình ảnh và dữ liệu gốc được lưu trên **Amazon S3**.

+ **Notification Layer**  
Nếu phát hiện sự cố có mức độ nguy hiểm cao, hệ thống sẽ gửi Email thông qua **Amazon SES** và SMS thông qua **Amazon SNS**.

+ **Monitoring Layer**  
Toàn bộ Logs, Metrics và Alarm được quản lý bởi **Amazon CloudWatch** nhằm hỗ trợ giám sát và vận hành hệ thống.

---

#### Kiến trúc triển khai

Trong workshop này, bạn sẽ triển khai các thành phần sau:

+ **Frontend VPC**
  + Amazon S3
  + Amazon CloudFront
  + Amazon Cognito

+ **Application Layer**
  + Amazon API Gateway
  + AWS Lambda

+ **AI Workflow**
  + Amazon EventBridge
  + AWS Step Functions
  + Amazon Rekognition
  + Amazon Bedrock

+ **Data Storage**
  + Amazon DynamoDB
  + Amazon S3

+ **Notification**
  + Amazon SNS
  + Amazon SES

+ **Monitoring & Security**
  + Amazon CloudWatch
  + IAM Roles
  + AWS Secrets Manager

Workshop này được xây dựng nhằm giúp người học làm quen với kiến trúc **Serverless**, **Event-Driven**, **AI Services** và các nguyên tắc thiết kế theo **AWS Well-Architected Framework**.

---

![Smart Campus Architecture](/images/5-Workshop/5.1/smart-campus-architecture.png)