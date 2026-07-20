---
title : "Tổng kết"
date : 2025-07-15
weight : 21
chapter : true
pre : " <b> 5.21. </b> "
---

# Tổng kết

## Giới thiệu

Xin chúc mừng!

Bạn đã hoàn thành Workshop **Smart Campus Guardian – AI Campus Incident Detection Platform** trên nền tảng **Amazon Web Services (AWS)**.

Thông qua Workshop này, bạn đã triển khai thành công một hệ thống giám sát an toàn khuôn viên trường học theo kiến trúc **Cloud Native**, **Serverless** và **Event-Driven**, kết hợp các dịch vụ AI của AWS để tự động phát hiện, phân tích và cảnh báo các sự cố.

---

# Các dịch vụ AWS đã triển khai

Trong Workshop, chúng ta đã sử dụng các dịch vụ sau:

+ AWS Identity and Access Management (IAM)

+ Amazon S3

+ Amazon CloudFront

+ Amazon Cognito

+ Amazon API Gateway

+ AWS Lambda

+ Amazon EventBridge

+ AWS Step Functions

+ Amazon Rekognition

+ Amazon Bedrock

+ Amazon DynamoDB

+ Amazon SNS

+ Amazon SES

+ Amazon CloudWatch

---

# Kiến trúc hoàn chỉnh

Toàn bộ hệ thống sau khi triển khai sẽ hoạt động theo kiến trúc dưới đây.

![Final Architecture](/images/5-Workshop/5.21/5.21.1/final-architecture.png)

---

## Luồng hoạt động của hệ thống

Quy trình xử lý hoàn chỉnh bao gồm:

1. Người quản trị truy cập Dashboard thông qua Amazon CloudFront.

2. Website React được tải từ Amazon S3.

3. Người dùng đăng nhập bằng Amazon Cognito.

4. Dashboard gửi yêu cầu đến Amazon API Gateway.

5. AWS Lambda xử lý các yêu cầu từ Dashboard.

6. Camera tải hình ảnh lên Amazon S3.

7. Amazon EventBridge phát hiện sự kiện mới.

8. AWS Step Functions khởi động AI Workflow.

9. AWS Lambda AI gọi Amazon Rekognition để phân tích hình ảnh.

10. Amazon Bedrock đánh giá mức độ rủi ro và sinh báo cáo AI.

11. AWS Lambda AI lưu Incident vào Amazon DynamoDB.

12. Amazon SNS gửi cảnh báo theo thời gian thực.

13. Amazon SES gửi Email thông báo.

14. Amazon CloudWatch ghi Logs, Metrics và giám sát toàn bộ hệ thống.

---

# Những kiến thức đạt được

Sau khi hoàn thành Workshop, bạn đã có thể:

✔ Thiết kế kiến trúc Cloud Native trên AWS.

✔ Triển khai Serverless Architecture.

✔ Xây dựng Event-Driven Architecture.

✔ Thiết kế AI Workflow với AWS Step Functions.

✔ Sử dụng Amazon Rekognition để phân tích hình ảnh.

✔ Sử dụng Amazon Bedrock để đánh giá mức độ rủi ro.

✔ Xây dựng REST API bằng Amazon API Gateway.

✔ Phát triển Backend bằng AWS Lambda.

✔ Quản lý người dùng bằng Amazon Cognito.

✔ Lưu trữ dữ liệu bằng Amazon DynamoDB.

✔ Gửi cảnh báo bằng Amazon SNS và Amazon SES.

✔ Giám sát hệ thống bằng Amazon CloudWatch.

✔ Triển khai Frontend ReactJS trên Amazon S3 và CloudFront.

---

# Hướng phát triển

Trong tương lai, hệ thống có thể được mở rộng theo nhiều hướng:

+ Amazon Kinesis Video Streams để xử lý video trực tiếp thay vì ảnh tĩnh.

+ AWS IoT Core để kết nối Camera và các cảm biến IoT.

+ Amazon OpenSearch Service phục vụ tìm kiếm và phân tích Incident.

+ Amazon QuickSight xây dựng Dashboard phân tích dữ liệu.

+ Amazon ECS hoặc Amazon EKS để triển khai các mô hình AI tùy chỉnh.

+ Amazon SageMaker để huấn luyện mô hình AI chuyên biệt.

+ AWS WAF và AWS Shield tăng cường bảo mật cho hệ thống.

---

# Kết quả cuối cùng

Sau khi hoàn thành Workshop, bạn đã xây dựng thành công một hệ thống AI Campus Incident Detection Platform hoạt động hoàn toàn trên AWS.

Hệ thống đáp ứng đầy đủ các yêu cầu:

+ Quản lý người dùng.

+ Tiếp nhận dữ liệu từ Camera.

+ Phân tích hình ảnh bằng AI.

+ Đánh giá mức độ nguy hiểm.

+ Lưu trữ Incident.

+ Gửi cảnh báo theo thời gian thực.

+ Giám sát toàn bộ hệ thống.

![Workshop Result](/images/5-Workshop/5.21/5.21.2/workshop-result.png)

---

# Kết luận

Workshop **Smart Campus Guardian – AI Campus Incident Detection Platform** là một ví dụ hoàn chỉnh về việc xây dựng hệ thống AI trên nền tảng AWS theo mô hình **Cloud Native**, **Serverless** và **Event-Driven**.

Thông qua Workshop này, bạn đã thực hành triển khai một kiến trúc hiện đại sử dụng các dịch vụ AWS từ quản lý người dùng, xử lý API, điều phối Workflow, trí tuệ nhân tạo, lưu trữ dữ liệu, gửi cảnh báo cho đến giám sát hệ thống.

Kiến trúc được xây dựng theo các nguyên tắc của **AWS Well-Architected Framework**, đảm bảo tính bảo mật, khả năng mở rộng, hiệu năng, độ tin cậy và tối ưu chi phí.

Xin cảm ơn bạn đã tham gia Workshop!