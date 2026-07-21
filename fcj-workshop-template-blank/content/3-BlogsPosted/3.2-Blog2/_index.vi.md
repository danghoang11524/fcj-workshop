---
title: "Blog 2"
date: 2026-07-21
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Từ API mở toang đến bắt buộc đăng nhập: Hành trình bảo mật Smart Notes API

Ở bài blog trước mình đã kể về việc xây dựng REST API Smart Notes trên kiến trúc Serverless. Sau khi deploy xong và test CRUD chạy ngon, mình nhận ra một vấn đề khá hiển nhiên nhưng dễ bị bỏ qua: API đang **mở hoàn toàn công khai** — ai có URL cũng gọi được, không giới hạn gì cả. Bài này chia sẻ lại 2 lớp bảo mật mình đã thêm vào.

## Lớp 1: API Key + Usage Plan

Việc đầu tiên và đơn giản nhất: thêm API Key ở tầng API Gateway. Trong `template.yaml`, chỉ cần thêm:

```yaml
SmartNotesApi:
  Type: AWS::Serverless::Api
  Properties:
    Auth:
      ApiKeyRequired: true
```

Kèm theo một Usage Plan giới hạn tốc độ gọi (throttle 10 request/giây) và quota (5000 request/tháng) — vừa chặn bot quét ngẫu nhiên, vừa tránh phát sinh chi phí ngoài ý muốn.

Điều học được ở bước này: **API Key không phải là xác thực người dùng**. Vì frontend là 1 file HTML tĩnh, ai mở DevTools/View Source cũng thấy được key. Nó chỉ chặn truy cập ngẫu nhiên và giới hạn tốc độ — không phân biệt được "ai" đang gọi API.

## Lớp 2: Cognito Hosted UI + đăng nhập Google

Vì API Key chưa đủ, mình thêm **Amazon Cognito** để bắt buộc đăng nhập thật trước khi truy cập được ghi chú. Chọn Cognito Hosted UI — trang đăng nhập/đăng ký có sẵn của AWS, hỗ trợ:

* Đăng ký/đăng nhập bằng email + mật khẩu
* Đăng nhập bằng Google (qua Google Identity Provider)

Không cần tự code màn hình login, chỉ khai báo User Pool, domain Hosted UI, liên kết Google OAuth Client (tạo bên Google Cloud Console) trong `template.yaml`. Ở API Gateway, gắn thêm Cognito Authorizer — nhưng chỉ áp dụng cho route `/notes*`, còn route phục vụ trang HTML tĩnh vẫn để công khai. Lý do: nếu chặn luôn cả trang HTML, người dùng sẽ không load được trang để... bấm nút đăng nhập — một vòng lặp bế tắc suýt mắc phải.

```yaml
Auth:
  ApiKeyRequired: true
  Authorizers:
    CognitoAuthorizer:
      UserPoolArn: !GetAtt SmartNotesUserPool.Arn
  DefaultAuthorizer: CognitoAuthorizer
```

Ở frontend, sau khi đăng nhập qua Hosted UI, Cognito redirect về kèm `id_token` trên URL. Token này lưu vào `localStorage`, gắn vào header `Authorization` cho mọi request gọi `/notes`, và tự động đưa người dùng quay lại trang đăng nhập nếu token hết hạn.

## Một vài vấp phải trong lúc làm

* Redirect URI phải khớp chính xác từng ký tự giữa Google Console, Cognito App Client và code frontend — thừa/thiếu dấu `/` cuối là bị từ chối ngay.
* Google yêu cầu cấu hình cả OAuth consent screen (Audience/Test users) trước khi tạo được OAuth Client — bước dễ bị bỏ sót nếu chỉ làm theo hướng dẫn cũ.
* Dùng tham số `NoEcho` cho Client Secret nên SAM CLI không lưu lại giá trị — mỗi lần deploy phải nhập tay hoặc truyền qua `--parameter-overrides` (liên quan trực tiếp đến CI/CD ở bài blog tiếp theo).

## Kết quả

Sau 2 lớp bảo mật này, Smart Notes API vừa chống được truy cập/spam vô danh (API Key), vừa bắt buộc người dùng đăng nhập thật (Cognito + Google) mới thao tác được với ghi chú. Bài blog tiếp theo sẽ kể về phần vận hành: tự động hóa deploy bằng CI/CD và bảo vệ dữ liệu bằng Point-in-Time Recovery.
**Xem thêm:**
* [Bài blog 1: Xây dựng Smart Notes API](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2220328232065470/?rdid=8cYdNHkX01xL0SYL#)
* [Bài blog 3: CI/CD tự động và bảo vệ dữ liệu bằng PITR](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2220307175400909/?rdid=4S6HwU2Sbma5XR0i#)
* [Bài viết gốc trên Facebook (AWS Study Group FCJ)](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2220307962067497/?rdid=6eWJUTaHaxuNrQHd#)
* [Source code trên GitHub](https://github.com/<username>/<repo>)