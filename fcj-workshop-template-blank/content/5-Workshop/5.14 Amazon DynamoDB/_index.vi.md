---
title : "Triển khai Amazon DynamoDB"
date : 2025-07-15
weight : 14
chapter : true
pre : " <b> 5.14. </b> "
---

# Triển khai Amazon DynamoDB

## Giới thiệu

Amazon DynamoDB là dịch vụ cơ sở dữ liệu NoSQL được quản lý hoàn toàn (Fully Managed) của AWS, có khả năng mở rộng tự động và cung cấp hiệu năng cao với độ trễ thấp.

Trong dự án **Smart Campus Guardian – AI Campus Incident Detection Platform**, Amazon DynamoDB được sử dụng để lưu trữ toàn bộ thông tin sự cố (Incident) sau khi Amazon Bedrock hoàn thành việc phân tích AI.

Lambda AI sẽ nhận kết quả từ Amazon Bedrock, chuẩn hóa dữ liệu và ghi vào bảng DynamoDB để Dashboard có thể truy vấn và hiển thị theo thời gian thực.

---

## Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ:

+ Tạo bảng Incident.
+ Thiết lập chế độ On-Demand.
+ Lưu kết quả AI từ Lambda AI.
+ Truy vấn dữ liệu cho Dashboard.

---

# Kiến trúc Amazon DynamoDB

Amazon DynamoDB là nơi lưu trữ toàn bộ dữ liệu sự cố sau khi AI hoàn thành phân tích.

Lambda AI sẽ ghi dữ liệu vào DynamoDB và Dashboard sẽ đọc dữ liệu để hiển thị cho người quản trị.

![DynamoDB Architecture](/images/5-Workshop/5.14/5.14.1/dynamodb.png)

---

## Giải thích kiến trúc

Quy trình hoạt động như sau:

1. AWS Step Functions điều phối AI Workflow.
2. Lambda AI nhận kết quả từ Amazon Bedrock.
3. Lambda AI chuẩn hóa dữ liệu Incident.
4. Lambda AI sử dụng API **PutItem** để ghi dữ liệu vào Amazon DynamoDB.
5. Dashboard thông qua API Gateway và Lambda sẽ sử dụng API **Scan** hoặc **Query** để đọc dữ liệu.
6. Amazon CloudWatch ghi lại Logs và Metrics của toàn bộ quá trình.

Việc sử dụng DynamoDB giúp hệ thống có khả năng mở rộng tự động và xử lý hàng nghìn sự kiện mỗi ngày mà không cần quản trị máy chủ cơ sở dữ liệu.

---

# Thiết kế bảng

Table Name

```
Incident
```

Partition Key

```
IncidentId (String)
```

Các thuộc tính lưu trữ:

```
CameraId
Location
RiskLevel
Status
CreatedAt
Summary
ImageUrl
RecommendedAction
```

---

# Các bước triển khai

## Bước 1: Mở Amazon DynamoDB

Đăng nhập **AWS Management Console**.

Tìm kiếm:

```
Amazon DynamoDB
```

Chọn:

```
Create Table
```

---

## Bước 2: Tạo bảng

Table Name

```
Incident
```

Partition Key

```
IncidentId
```

Type

```
String
```

![Create Table](/images/5-Workshop/5.14/5.14.2/create-table.png)

---

## Bước 3: Chọn Billing Mode

Billing Mode

```
On-Demand
```

Chế độ này phù hợp với Workshop vì không cần dự đoán trước lưu lượng truy cập và chỉ trả phí theo số lượng request thực tế.

---

## Bước 4: Tạo bảng

Nhấn

```
Create Table
```

Đợi đến khi trạng thái bảng chuyển sang:

```
Active
```

---

## Bước 5: Lưu dữ liệu từ Lambda AI

Sau khi Amazon Bedrock trả về kết quả phân tích, Lambda AI sẽ sử dụng API:

```
PutItem
```

để ghi dữ liệu vào bảng:

```
Incident
```

Ví dụ dữ liệu:

```json
{
  "IncidentId": "INC-0001",
  "CameraId": "CAM-01",
  "RiskLevel": "HIGH",
  "Status": "OPEN",
  "Summary": "Fire detected near Building A.",
  "RecommendedAction": "Notify campus security immediately.",
  "CreatedAt": "2025-07-15T10:30:00Z",
  "ImageUrl": "s3://smart-campus-images/fire.jpg"
}
```

---

## Bước 6: Kiểm tra

Upload hình ảnh:

```
fire.jpg
```

Sau khi AI Workflow hoàn thành:

```
AWS Lambda

↓

Amazon DynamoDB

↓

Items

↓

Incident
```

Kiểm tra dữ liệu đã được lưu thành công.

---

## Best Practices

✔ Sử dụng chế độ **On-Demand** để tối ưu chi phí.

✔ Chỉ lưu URL của hình ảnh, không lưu trực tiếp file trong cơ sở dữ liệu.

✔ Thiết kế Partition Key ngắn gọn và duy nhất.

✔ Chuẩn hóa dữ liệu trước khi ghi vào DynamoDB.

✔ Ghi Logs bằng Amazon CloudWatch.

---

## Kiểm tra

Sau khi hoàn thành chương này, bạn sẽ có:

+ Bảng DynamoDB Incident.

+ Lambda AI ghi dữ liệu thành công.

+ Dashboard có thể truy vấn dữ liệu.

+ Incident được lưu trữ đầy đủ.

---

## Kết quả

Trong chương này, bạn đã triển khai thành công Amazon DynamoDB để lưu trữ toàn bộ thông tin sự cố của hệ thống **Smart Campus Guardian**.

Amazon DynamoDB đóng vai trò là cơ sở dữ liệu trung tâm, giúp Dashboard truy vấn nhanh các sự cố và hỗ trợ các dịch vụ AWS khác trong việc theo dõi trạng thái của hệ thống.

Trong chương tiếp theo, chúng ta sẽ triển khai **Amazon SNS** để gửi cảnh báo theo thời gian thực khi phát hiện sự cố có mức độ nguy hiểm cao.