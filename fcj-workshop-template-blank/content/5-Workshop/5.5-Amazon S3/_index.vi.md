---
title : "Triển khai Amazon S3"
date : 2025-07-14
weight : 5
chapter : true
pre : " <b> 5.5. </b> "
---

# Triển khai Amazon S3

## Giới thiệu

Amazon Simple Storage Service (Amazon S3) là dịch vụ lưu trữ đối tượng (Object Storage) của AWS với khả năng mở rộng gần như không giới hạn.

Trong dự án **Smart Campus Guardian – AI Campus Incident Detection Platform**, Amazon S3 đóng vai trò là điểm khởi đầu của toàn bộ quy trình xử lý AI. Khi Camera tải hình ảnh lên S3, hệ thống sẽ tự động kích hoạt các dịch vụ AWS để phân tích hình ảnh và gửi cảnh báo nếu phát hiện sự cố.

---

## Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ:

- Tạo Bucket lưu trữ hình ảnh từ Camera.
- Tạo Bucket lưu trữ Website React.
- Tạo Bucket lưu trữ báo cáo AI.
- Bật Versioning cho Bucket.
- Bật Server-side Encryption.
- Chuẩn bị Bucket để kích hoạt EventBridge ở chương tiếp theo.

---

# Kiến trúc Amazon S3

Amazon S3 là nơi lưu trữ toàn bộ dữ liệu của hệ thống và là thành phần đầu tiên trong AI Workflow.

Khi Camera tải hình ảnh lên Bucket, Amazon S3 sẽ phát sinh sự kiện **Object Created**, từ đó kích hoạt Amazon EventBridge và AWS Step Functions để xử lý AI.

![Amazon S3 Architecture](/images/5-Workshop/5.5/5.5.1/s3-architecture.png)

---

## Giải thích kiến trúc

Hệ thống sử dụng ba Bucket chính:

| Bucket | Chức năng |
|---------|-----------|
| **smart-campus-images** | Lưu hình ảnh từ Camera |
| **smart-campus-frontend** | Lưu Website React |
| **smart-campus-report** | Lưu báo cáo AI và kết quả phân tích |

Trong Workshop này, Bucket **smart-campus-images** là thành phần quan trọng nhất vì sẽ được sử dụng để kích hoạt AI Workflow ở các chương tiếp theo.

---

## Bước 1: Tạo Bucket lưu hình ảnh

Đăng nhập **AWS Management Console**.

Tìm kiếm:

```
Amazon S3
```

Chọn:

```
Create Bucket
```

Cấu hình:

Bucket Name

```
smart-campus-images
```

Region

```
Asia Pacific (Singapore)
ap-southeast-1
```

Block Public Access

```
Keep all settings enabled
```

![Create Bucket](/images/5-Workshop/5.5/5.5.2/create-bucket.png)

---

## Bước 2: Tạo Bucket Website

Tạo Bucket mới với tên:

```
smart-campus-frontend
```

Bucket này sẽ được sử dụng để triển khai Website React ở chương Dashboard.

---

## Bước 3: Tạo Bucket lưu AI Report

Tạo Bucket mới:

```
smart-campus-report
```

Bucket này sẽ lưu:

- Báo cáo AI
- Kết quả phân tích
- Dữ liệu xuất từ Amazon Bedrock

---

## Bước 4: Bật Versioning

Trong Bucket:

```
Properties
        ↓
Bucket Versioning
        ↓
Enable
```

Versioning giúp khôi phục dữ liệu nếu người dùng vô tình ghi đè hoặc xóa đối tượng.

---

## Bước 5: Bật Server-side Encryption

Trong Bucket:

```
Properties
        ↓
Default Encryption
        ↓
Server-side Encryption (SSE-S3)
```

Việc mã hóa giúp bảo vệ dữ liệu được lưu trữ trên Amazon S3.

---

## Bước 6: Upload hình ảnh thử nghiệm

Tải lên một hình ảnh để kiểm tra Bucket.

Ví dụ:

```
fire.jpg
```

Sau khi upload thành công, Bucket sẽ hiển thị đối tượng mới.

![Upload Image](/images/5-Workshop/5.5/5.5.3/upload-image.png)

---

## Best Practices

Để đảm bảo an toàn và tối ưu chi phí, hãy áp dụng các nguyên tắc sau:

- Giữ nguyên Block Public Access.
- Bật Versioning cho các Bucket quan trọng.
- Bật Server-side Encryption.
- Không lưu thông tin nhạy cảm trong Bucket công khai.
- Phân tách Bucket theo từng chức năng của hệ thống.

---

## Kết quả

Sau khi hoàn thành chương này, bạn đã:

- Tạo Bucket **smart-campus-images**.
- Tạo Bucket **smart-campus-frontend**.
- Tạo Bucket **smart-campus-report**.
- Bật Versioning.
- Bật Server-side Encryption.
- Upload thành công hình ảnh thử nghiệm.

Các Bucket này sẽ được sử dụng trong những chương tiếp theo để xây dựng AI Workflow và triển khai toàn bộ hệ thống Smart Campus Guardian.

Trong chương tiếp theo, chúng ta sẽ cấu hình **AI Workflow** để Amazon S3 tự động kích hoạt quy trình xử lý khi có hình ảnh mới được tải lên.