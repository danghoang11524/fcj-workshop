---
title: "Nhật ký công việc"
date: 2026-07-25
weight: 1
chapter: true
pre: " <b> 1. </b> "
---

Worklog này được thực hiện trong khoảng **12 tuần (từ 04/05/2026 đến 27/07/2026)**, trong suốt thời gian thực tập, với mục tiêu xây dựng hoàn chỉnh hệ thống backend **Smart Notes API** — một REST API quản lý ghi chú theo kiến trúc **Serverless hoàn toàn** trên AWS, có xác thực người dùng, bảo mật nhiều lớp, sao lưu dữ liệu và tự động hóa triển khai. Nội dung các tuần như sau:

**Tuần 1:** [Orientation & AWS Basics](1.1-week1/) — Làm quen với AWS, tạo tài khoản/IAM User, tìm hiểu các dịch vụ cốt lõi sẽ sử dụng (Lambda, API Gateway, DynamoDB, S3, Cognito), cài đặt AWS CLI/SAM CLI và lên kế hoạch kiến trúc tổng thể cho dự án.

**Tuần 2:** [IAM, S3 & CLI](1.2-week2/) — Thiết lập IAM Role/Policy theo nguyên tắc Least Privilege, tạo bucket S3 để lưu trữ ảnh đính kèm ghi chú, thực hành thao tác qua AWS CLI và làm quen với AWS SAM để chuẩn bị cho việc định nghĩa hạ tầng dưới dạng code.

**Tuần 3:** [Lambda & API Gateway](1.3-week3/) — Xây dựng function AWS Lambda đầu tiên (Node.js 22, Express + serverless-http), kết nối với Amazon API Gateway, cấu hình route REST cơ bản và kiểm thử bằng Postman.

**Tuần 4:** [CRUD API - Notes](1.4-week4/) — Triển khai đầy đủ các endpoint CRUD (tạo, xem, sửa, xóa ghi chú) theo mô hình Clean Architecture (Controller → Service → Repository), tích hợp upload/xóa ảnh lên S3 kèm cơ chế retry khi upload lỗi.

**Tuần 5:** [Authentication - Cognito](1.5-week5/) — Tích hợp Amazon Cognito User Pool và Hosted UI, cấu hình đăng nhập bằng email/mật khẩu và liên kết đăng nhập Google qua Google Identity Provider, áp dụng Cognito Authorizer để bắt buộc xác thực trước khi gọi API.

**Tuần 6:** [DynamoDB Integration](1.6-week6/) — Thiết kế bảng Notes trên Amazon DynamoDB với khóa chính `userId` + `noteId` để cách ly dữ liệu giữa các người dùng, chuyển đổi tầng Repository sang thao tác với DynamoDB ở chế độ PAY_PER_REQUEST.

**Tuần 7:** [Advanced API Features](1.7-week7/) — Bổ sung các tính năng nâng cao: phân trang, tìm kiếm phía backend, gắn tag và ghim ghi chú; tối ưu response format và validation dữ liệu đầu vào.

**Tuần 8:** [Deployment & CI/CD](1.8-week8/) — Đóng gói toàn bộ hạ tầng bằng AWS SAM (`template.yaml`), thiết lập pipeline CI/CD để tự động `sam build && sam deploy` mỗi khi push code lên Git, giảm thiểu lỗi do triển khai thủ công.

**Tuần 9:** [Logging & Monitoring](1.9-week9/) — Cấu hình Amazon CloudWatch Logs cho từng Lambda, xây dựng quy trình tra cứu lỗi và theo dõi hiệu năng hệ thống theo thời gian thực.

**Tuần 10:** [Optimization & Security](1.10-week10/) — Bổ sung API Key và Usage Plan để giới hạn tốc độ gọi API (throttle/quota) chống lạm dụng, bật Point-in-Time Recovery (PITR) cho DynamoDB để bảo vệ dữ liệu, rà soát lại IAM permissions và thiết lập Billing Alarm.

**Tuần 11:** [Testing & Documentation](1.11-week11/) — Kiểm thử toàn diện hệ thống (chức năng, bảo mật, khôi phục dữ liệu bằng PITR, kiểm thử tải với Artillery/k6), hoàn thiện tài liệu kỹ thuật và hướng dẫn sử dụng.

**Tuần 12:** [Final Demo & Report](1.12-week12/) — Hoàn thiện giao diện web tĩnh kết nối với API, tổng hợp toàn bộ quá trình thực hiện, chuẩn bị demo hệ thống và viết báo cáo tổng kết thực tập.