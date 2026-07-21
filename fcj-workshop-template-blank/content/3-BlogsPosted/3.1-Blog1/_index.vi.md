---
title: "Blog 1"
date: 2026-07-21
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Hành trình xây dựng Smart Notes API — REST API Serverless đầu tiên của mình trên AWS

Trong đợt thực tập vừa rồi, mình đã tự tay xây dựng và triển khai một dự án cá nhân: **Smart Notes API** — một REST API quản lý ghi chú (notes), chạy hoàn toàn theo kiến trúc Serverless trên AWS. Bài viết này chia sẻ lại toàn bộ hành trình, từ lý do chọn kiến trúc đến những gì đã thực sự chạy được.

## Vì sao lại là Serverless?

Trước khi bắt tay vào làm, mình có 2 lựa chọn: dựng 1 con EC2 chạy Node.js 24/7, hoặc đi theo hướng Serverless. Mình chọn Serverless vì lý do rất thực tế: đây là dự án cá nhân, traffic gần như bằng 0 phần lớn thời gian trong ngày — không có lý do gì để trả tiền cho một server "ngồi không" cả tháng. Serverless giúp mình chỉ trả tiền đúng lúc có request thật.

## Kiến trúc mình chọn

```
Client (HTML tĩnh) → Amazon API Gateway → AWS Lambda (Express + serverless-http)
                                              │
                                    ┌─────────┴─────────┐
                                    ▼                   ▼
                              Amazon DynamoDB      Amazon S3
                              (dữ liệu note)       (ảnh đính kèm)
```

Lambda chạy Express thông qua thư viện `serverless-http` — nghĩa là mình vẫn viết code Express quen thuộc (route, middleware, controller) như một server bình thường, nhưng khi deploy thì nó chạy dưới dạng Lambda function, không có server nào tồn tại thường trực cả.

Để code không bị rối khi dự án lớn dần, mình tổ chức theo **Clean Architecture**: Controller (nhận request) → Service (xử lý nghiệp vụ, có retry khi upload ảnh S3 lỗi) → Repository (thao tác với DynamoDB). Tách lớp rõ ràng giúp viết unit test (Jest) dễ hơn hẳn — mock được DynamoDB/S3 mà không cần gọi AWS thật khi chạy test.

## Toàn bộ hạ tầng là code — AWS SAM

Một điều khá tâm đắc: từ đầu đến cuối, mình không hề click tay trên Console để tạo tài nguyên. Toàn bộ DynamoDB table, S3 bucket, Lambda function, API Gateway, IAM Role đều được khai báo trong đúng 1 file `template.yaml`, deploy bằng:

```bash
sam build && sam deploy --guided
```

Cái hay của cách này: nếu lỡ tay xóa nhầm cả stack, chỉ cần chạy lại đúng lệnh trên là có lại y hệt hạ tầng cũ — không cần nhớ đã click những gì trên Console.

## Frontend tối giản

Vì là dự án cá nhân nên mình không dựng React/Vue gì cả — chỉ 1 file HTML/CSS/JS thuần, gọi thẳng API qua `fetch`. Giao diện lấy cảm hứng từ "thẻ mục lục thư viện" (card catalog) — mỗi note là 1 tấm thẻ giấy kem, có con dấu "FILED" khi lưu thành công, khá vui mắt cho một demo cá nhân.

## Kết quả

Sau khi hoàn thành, mình có một REST API chạy thật, hỗ trợ đầy đủ CRUD, upload/xóa ảnh, và một giao diện web dùng được ngay. Đây mới chỉ là bước đầu — ở 2 bài blog tiếp theo mình sẽ kể tiếp về phần bảo mật (API Key, Cognito, đăng nhập Google) và phần vận hành thực chiến (CI/CD, backup dữ liệu) mà mình đã thêm vào sau đó.
**Xem thêm:**
* [Bài blog 2: Bảo mật với API Key và Cognito](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2220307962067497/?rdid=St8Z9r4R01fFMtL7#)
* [Bài blog 3: CI/CD tự động và bảo vệ dữ liệu bằng PITR](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2220307175400909/?rdid=4S6HwU2Sbma5XR0i#)
* [Bài viết gốc trên Facebook (AWS Study Group FCJ)](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2220328232065470/?rdid=dfMOxtzDfBog3SYU#)
