---
title : "Triển khai Amazon Rekognition"
date : 2025-07-15
weight : 12
chapter : true
pre : " <b> 5.12. </b> "
---

# Triển khai Amazon Rekognition

## Giới thiệu

Amazon Rekognition là dịch vụ AI của AWS giúp phân tích hình ảnh và phát hiện các đối tượng bằng công nghệ Machine Learning.

Trong dự án **Smart Campus Guardian – AI Campus Incident Detection Platform**, Amazon Rekognition được sử dụng để nhận diện các đối tượng xuất hiện trong hình ảnh do Camera tải lên Amazon S3.

Sau khi AWS Step Functions kích hoạt AI Workflow, Lambda AI sẽ gửi hình ảnh đến Amazon Rekognition để phân tích. Kết quả sau đó sẽ được chuyển sang Amazon Bedrock để đánh giá mức độ nguy hiểm và đưa ra khuyến nghị xử lý.

---

## Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ:

+ Kết nối AWS Lambda với Amazon Rekognition.
+ Phân tích hình ảnh từ Amazon S3.
+ Nhận diện các đối tượng trong ảnh.
+ Trả về danh sách Labels và Confidence Score.
+ Chuẩn bị dữ liệu cho Amazon Bedrock.

---

# Kiến trúc Amazon Rekognition

Amazon Rekognition là dịch vụ AI chịu trách nhiệm nhận diện hình ảnh trong AI Workflow của Smart Campus Guardian.

Lambda AI sẽ gửi hình ảnh đến Amazon Rekognition để phân tích trước khi chuyển kết quả sang Amazon Bedrock.

![Rekognition Architecture](/images/5-Workshop/5.12/5.12.1/ekognition-architecture.png)

---

## Giải thích kiến trúc

Quy trình xử lý hoạt động như sau:

1. AWS Step Functions gọi Lambda AI.
2. Lambda AI đọc hình ảnh từ Amazon S3.
3. Lambda AI gửi hình ảnh đến Amazon Rekognition.
4. Amazon Rekognition phát hiện các đối tượng và trả về Labels cùng Confidence Score.
5. Lambda AI nhận kết quả và chuyển sang Amazon Bedrock để phân tích ngữ cảnh.
6. CloudWatch ghi lại toàn bộ Logs và Metrics.

Việc sử dụng Amazon Rekognition giúp hệ thống tự động nhận diện các đối tượng quan trọng trước khi AI đánh giá mức độ rủi ro.

---

# Các đối tượng nhận diện

Trong Workshop này, Amazon Rekognition sẽ phát hiện các đối tượng như:

+ Person

+ Fire

+ Smoke

+ Vehicle

+ Bicycle

+ Backpack

+ Helmet

+ Crowd

Ngoài ra, Rekognition còn có thể mở rộng để nhận diện nhiều đối tượng khác tùy theo yêu cầu của hệ thống.

---

# Các bước triển khai

## Bước 1: Mở Amazon Rekognition

Đăng nhập **AWS Management Console**.

Tìm kiếm:

```
Amazon Rekognition
```

Chọn:

```
Image Analysis
```

---

## Bước 2: Chuẩn bị hình ảnh

Sử dụng hình ảnh đã tải lên Bucket:

```
smart-campus-images
```

Ví dụ:

```
fire.jpg
```

---

## Bước 3: Cấu hình Detect Labels

Trong Lambda AI, sử dụng API:

```
DetectLabels
```

Confidence Threshold

```
90%
```

Max Labels

```
10
```

![Detect Labels](/images/5-Workshop/5.12/5.12.2/create-rekognition.png)

---

## Bước 4: Phân tích kết quả

Ví dụ kết quả trả về:

```
Fire
Confidence: 98%

Smoke
Confidence: 96%

Person
Confidence: 99%
```

Lambda AI sẽ lọc các Labels cần thiết trước khi gửi sang Amazon Bedrock.

---

## Bước 5: Chuyển dữ liệu sang Amazon Bedrock

Các Labels sau khi được xử lý sẽ được chuyển đến Amazon Bedrock để:

+ Đánh giá mức độ nguy hiểm.

+ Phân loại Incident.

+ Sinh nội dung cảnh báo.

+ Đề xuất phương án xử lý.

---

## Bước 6: Kiểm tra

Upload một hình ảnh:

```
fire.jpg
```

Kiểm tra:

```
CloudWatch

↓

Lambda Logs

↓

Amazon Rekognition Response
```

Nếu thành công, Lambda sẽ nhận được danh sách Labels và Confidence Score.

---

## Best Practices

Để đảm bảo độ chính xác của hệ thống, nên áp dụng các khuyến nghị sau:

✔ Chỉ sử dụng Confidence từ 90% trở lên.

✔ Lọc bỏ các Labels không liên quan.

✔ Không lưu toàn bộ kết quả Rekognition vào cơ sở dữ liệu.

✔ Chỉ chuyển các Labels cần thiết sang Amazon Bedrock.

✔ Ghi Logs bằng Amazon CloudWatch.

---

## Kiểm tra

Sau khi hoàn thành chương này, bạn sẽ có:

+ Amazon Rekognition hoạt động.

+ Lambda AI kết nối thành công với Rekognition.

+ Danh sách Labels.

+ Confidence Score.

+ Dữ liệu sẵn sàng cho Amazon Bedrock.

---

## Kết quả

Trong chương này, bạn đã triển khai thành công Amazon Rekognition để nhận diện các đối tượng trong hình ảnh của hệ thống **Smart Campus Guardian**.

Amazon Rekognition giúp phát hiện các đối tượng quan trọng như người, lửa, khói và phương tiện, từ đó cung cấp dữ liệu đầu vào cho Amazon Bedrock thực hiện phân tích ngữ cảnh và đánh giá mức độ nguy hiểm.

Trong chương tiếp theo, chúng ta sẽ triển khai **Amazon Bedrock** để xây dựng AI phân tích sự cố và đưa ra cảnh báo thông minh.