---
title : "Phân tích sự cố với Amazon Bedrock"
date : 2025-07-15
weight : 13
chapter : true
pre : " <b> 5.13. </b> "
---

# Phân tích sự cố với Amazon Bedrock

## Giới thiệu

Amazon Bedrock là dịch vụ Generative AI của AWS, cho phép sử dụng các mô hình ngôn ngữ lớn (Large Language Models - LLM) mà không cần tự quản lý hạ tầng Machine Learning.

Trong dự án **Smart Campus Guardian – AI Campus Incident Detection Platform**, Amazon Bedrock nhận kết quả phân tích từ Amazon Rekognition để đánh giá mức độ nguy hiểm của sự cố, tạo báo cáo AI và đề xuất phương án xử lý.

Sau khi AWS Step Functions điều phối quy trình và Lambda AI nhận được các Labels từ Amazon Rekognition, Lambda AI sẽ gửi dữ liệu sang Amazon Bedrock để thực hiện phân tích ngữ cảnh trước khi lưu kết quả vào Amazon DynamoDB.

---

## Mục tiêu

Sau khi hoàn thành chương này, bạn sẽ:

+ Kết nối Lambda AI với Amazon Bedrock.
+ Phân tích dữ liệu từ Amazon Rekognition.
+ Đánh giá mức độ nguy hiểm của sự cố.
+ Sinh báo cáo AI và khuyến nghị xử lý.
+ Chuẩn hóa dữ liệu Incident trước khi lưu vào DynamoDB.

---

# Kiến trúc Amazon Bedrock

Amazon Bedrock là thành phần AI chịu trách nhiệm phân tích ngữ cảnh và đánh giá mức độ nghiêm trọng của sự cố trong hệ thống Smart Campus Guardian.

Lambda AI sẽ gửi các Labels từ Amazon Rekognition sang Amazon Bedrock để tạo báo cáo AI trước khi lưu kết quả.

![Bedrock Architecture](/images/5-Workshop/5.13/5.13.1/bedrock-architecture.png)

---

## Giải thích kiến trúc

Quy trình xử lý hoạt động như sau:

1. AWS Step Functions gọi Lambda AI.
2. Lambda AI nhận kết quả từ Amazon Rekognition.
3. Lambda AI tạo Prompt và gửi sang Amazon Bedrock.
4. Amazon Bedrock phân tích ngữ cảnh bằng mô hình Generative AI.
5. Bedrock trả về:
   - Risk Level.
   - Incident Summary.
   - Recommended Action.
6. Lambda AI chuẩn hóa dữ liệu và lưu vào Amazon DynamoDB.
7. CloudWatch ghi lại Logs và Metrics của toàn bộ quá trình.

Việc sử dụng Amazon Bedrock giúp hệ thống không chỉ nhận diện đối tượng mà còn hiểu ngữ cảnh và đưa ra khuyến nghị xử lý phù hợp.

---

# Các bước triển khai

## Bước 1: Mở Amazon Bedrock

Đăng nhập **AWS Management Console**.

Tìm kiếm:

```
Amazon Bedrock
```

Chọn:

```
Model Playground
```

---

## Bước 2: Chọn Foundation Model

Model đề xuất:

```
Amazon Nova Lite
```

Hoặc:

```
Claude 3 Haiku
```

Chọn một Foundation Model để thực hiện phân tích.

![Model Playground](/images/5-Workshop/5.13/5.13.2/create-bedrock.png)

---

## Bước 3: Tạo Prompt

Ví dụ Prompt:

```text
You are an AI assistant for a Smart Campus Security System.

Detected objects:

Fire: 98%
Smoke: 95%
Person: 2

Analyze the incident and return JSON with:

- Risk Level
- Summary
- Recommended Action
```

---

## Bước 4: Kiểm tra kết quả

Ví dụ kết quả trả về:

```json
{
  "riskLevel": "HIGH",
  "summary": "Fire and smoke detected near Building A. Two people are present in the affected area.",
  "recommendedAction": "Notify campus security immediately and initiate evacuation procedures."
}
```

Lambda AI sẽ nhận kết quả này và chuẩn hóa dữ liệu trước khi lưu vào Amazon DynamoDB.

---

## Bước 5: Lưu Incident

Sau khi nhận phản hồi từ Amazon Bedrock:

```
Lambda AI

↓

Amazon DynamoDB
```

Thông tin được lưu bao gồm:

+ Incident ID
+ Timestamp
+ Risk Level
+ Summary
+ Recommended Action

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

Amazon Bedrock Response
```

Nếu thành công, Lambda AI sẽ nhận được báo cáo AI và lưu Incident vào Amazon DynamoDB.

---

## Best Practices

✔ Chỉ gửi các Labels cần thiết sang Amazon Bedrock.

✔ Không gửi toàn bộ hình ảnh nếu không cần thiết.

✔ Chuẩn hóa JSON Output.

✔ Sử dụng Prompt rõ ràng và dễ bảo trì.

✔ Ghi Logs bằng Amazon CloudWatch.

---

## Kiểm tra

Sau khi hoàn thành chương này, bạn sẽ có:

+ Amazon Bedrock hoạt động.

+ Lambda AI kết nối thành công với Bedrock.

+ Báo cáo AI.

+ Risk Level.

+ Recommended Action.

+ Dữ liệu sẵn sàng lưu vào Amazon DynamoDB.

---

## Kết quả

Trong chương này, bạn đã triển khai thành công Amazon Bedrock để phân tích ngữ cảnh và đánh giá mức độ nghiêm trọng của sự cố trong hệ thống **Smart Campus Guardian**.

Amazon Bedrock giúp chuyển đổi kết quả nhận diện từ Amazon Rekognition thành báo cáo AI có ý nghĩa, hỗ trợ người vận hành đưa ra quyết định nhanh chóng và chính xác.

Trong chương tiếp theo, chúng ta sẽ triển khai **Amazon DynamoDB** để lưu trữ toàn bộ thông tin sự cố do AI tạo ra.