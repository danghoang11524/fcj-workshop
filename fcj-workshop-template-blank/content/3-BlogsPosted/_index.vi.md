---
title: "Các bài blogs đã đăng"
date: 2026-07-21
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Ở phần này là 3 bài blog mình đã đăng lên [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj), chia sẻ lại toàn bộ hành trình xây dựng dự án cá nhân **Smart Notes API** — một REST API quản lý ghi chú chạy theo kiến trúc Serverless trên AWS.

### [Blog 1 - Hành trình xây dựng Smart Notes API](3.1-Blog1/)
Lý do chọn kiến trúc Serverless thay vì EC2, thiết kế hệ thống với API Gateway + Lambda (Express/serverless-http) + DynamoDB + S3, tổ chức code theo Clean Architecture, và triển khai toàn bộ hạ tầng dưới dạng code bằng AWS SAM.

### [Blog 2 - Từ API mở toang đến bắt buộc đăng nhập](3.2-Blog2/)
Hành trình bảo mật API: thêm API Key + Usage Plan ở tầng API Gateway để chặn truy cập/spam vô danh, sau đó tích hợp Amazon Cognito Hosted UI kèm đăng nhập Google để bắt buộc xác thực người dùng thật trước khi thao tác với ghi chú.

### [Blog 3 - Vận hành thực chiến: CI/CD tự động và bảo vệ dữ liệu bằng PITR](3.3-Blog3/)
Tự động hóa quy trình deploy bằng CI/CD với GitHub Actions (test luôn chạy trước deploy), và bảo vệ dữ liệu bằng Point-in-Time Recovery (PITR) của DynamoDB để có thể khôi phục dữ liệu về bất kỳ thời điểm nào trong 35 ngày gần nhất.