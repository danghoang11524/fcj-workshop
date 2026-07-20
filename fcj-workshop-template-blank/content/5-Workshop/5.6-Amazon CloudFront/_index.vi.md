---
title : "Triển khai Amazon CloudFront"
date : 2025-07-14
weight : 6
chapter : true
pre : " <b> 5.6. </b> "
---

# Triển khai Amazon CloudFront

## Giới thiệu

Amazon CloudFront là dịch vụ **Content Delivery Network (CDN)** của AWS, giúp phân phối nội dung đến người dùng thông qua hệ thống **Edge Locations** trên toàn cầu.

Trong dự án **Smart Campus Guardian – AI Campus Incident Detection Platform**, CloudFront được sử dụng để phân phối Website React được lưu trữ trên Amazon S3, giúp tăng tốc độ truy cập, hỗ trợ HTTPS và nâng cao tính bảo mật.

---

## Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ:

+ Tạo CloudFront Distribution.
+ Kết nối CloudFront với Amazon S3.
+ Cấu hình Origin Access Control (OAC).
+ Bật HTTPS cho Website.
+ Phân phối Website React trên toàn cầu.

---

# Kiến trúc CloudFront

CloudFront là lớp đầu tiên mà người dùng truy cập khi sử dụng hệ thống Smart Campus Guardian.

CloudFront sẽ nhận yêu cầu từ người dùng, sau đó phân phối nội dung từ Amazon S3 thông qua các Edge Locations gần nhất nhằm giảm độ trễ và tối ưu hiệu năng.

![CloudFront Architecture](/images/5-Workshop/5.6/5.6.1/cloudfront-architecture.png)

---

## Giải thích kiến trúc

Quy trình hoạt động của CloudFront:

1. Người dùng truy cập Website.
2. CloudFront kiểm tra nội dung trong Edge Cache.
3. Nếu dữ liệu đã được lưu trong Cache, CloudFront trả kết quả ngay lập tức.
4. Nếu chưa có, CloudFront sẽ lấy dữ liệu từ Amazon S3.
5. Nội dung được lưu lại tại Edge Location để phục vụ các lần truy cập tiếp theo.

Việc sử dụng CloudFront giúp:

+ Giảm thời gian tải Website.
+ Hỗ trợ HTTPS.
+ Giảm tải cho Amazon S3.
+ Tăng tính bảo mật thông qua Origin Access Control (OAC).

---

# Các bước triển khai

## Bước 1: Mở Amazon CloudFront

Đăng nhập **AWS Management Console**.

Tìm kiếm:

```
CloudFront
```

Chọn:

```
Create Distribution
```

---

## Bước 2: Cấu hình Origin

Origin Domain

```
smart-campus-frontend
```

Origin Type

```
Amazon S3
```

Origin Access

```
Origin Access Control (OAC)
```

CloudFront sẽ sử dụng OAC để truy cập Bucket Amazon S3 mà không cần Public Bucket.

![Create Distribution](/images/5-Workshop/5.6/5.6.2/create-distribution.png)

---

## Bước 3: Cấu hình Viewer

Viewer Protocol Policy

```
Redirect HTTP to HTTPS
```

Default Root Object

```
index.html
```

Price Class

```
Use All Edge Locations
```

Các thiết lập này giúp Website luôn sử dụng HTTPS và có thể truy cập nhanh từ mọi khu vực trên thế giới.

---

## Bước 4: Tạo Distribution

Kiểm tra lại toàn bộ cấu hình.

Chọn:

```
Create Distribution
```

Quá trình triển khai thường mất khoảng **10–15 phút**.

---

## Bước 5: Kiểm tra CloudFront

Sau khi Distribution được tạo thành công, CloudFront sẽ cung cấp một Domain Name.

Ví dụ:

```
d123abcxyz.cloudfront.net
```

Mở trình duyệt và truy cập Domain trên.

Nếu Website React hiển thị thành công, quá trình triển khai đã hoàn tất.

---

## Best Practices

Để đảm bảo hiệu năng và bảo mật, nên áp dụng các khuyến nghị sau:

✔ Sử dụng HTTPS cho toàn bộ Website.

✔ Sử dụng Origin Access Control (OAC).

✔ Không Public Amazon S3 Bucket.

✔ Bật Compression để giảm dung lượng truyền tải.

✔ Sử dụng Cache Policy mặc định của CloudFront.

✔ Sử dụng AWS Certificate Manager khi triển khai Domain riêng.

---

## Kiểm tra

Sau khi hoàn thành chương này, bạn sẽ có:

+ CloudFront Distribution hoạt động.

+ Website React được phân phối thông qua CloudFront.

+ HTTPS được kích hoạt.

+ Amazon S3 chỉ cho phép CloudFront truy cập thông qua OAC.

---

## Kết quả

Trong chương này, bạn đã triển khai thành công Amazon CloudFront để phân phối Website React của Smart Campus Guardian.

CloudFront đóng vai trò là lớp CDN phía trước Amazon S3, giúp tăng tốc độ truy cập, giảm độ trễ và nâng cao tính bảo mật cho hệ thống.

Trong chương tiếp theo, chúng ta sẽ triển khai **Amazon Cognito** để quản lý người dùng và xác thực truy cập trước khi gọi các API của hệ thống.