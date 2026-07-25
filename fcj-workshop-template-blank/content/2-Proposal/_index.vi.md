---
title: "Bản đề xuất"
date: 2026-07-25
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Smart Notes API

## Nền tảng REST API Quản lý Ghi chú theo kiến trúc Serverless

**Smart Notes API** là hệ thống quản lý ghi chú (notes) dạng REST API, được xây dựng theo kiến trúc **Serverless hoàn toàn** trên Amazon Web Services (AWS). Hệ thống cho phép người dùng tạo, xem, sửa, xóa ghi chú và đính kèm hình ảnh; đi kèm giao diện web tĩnh phong cách "thẻ mục lục thư viện" (card catalog); có xác thực đăng nhập (email/mật khẩu hoặc Google) và bảo mật API nhiều lớp ở tầng gateway.

| | |
|---|---|
| **Trạng thái dự án** | Đã hoàn thành |
---

# 1. Tóm tắt tổng quan

Các công cụ ghi chú cá nhân đơn giản thường buộc người dùng/đội phát triển phải đánh đổi giữa hai lựa chọn:

1. **Dùng dịch vụ SaaS có sẵn** — nhanh nhưng mất kiểm soát dữ liệu, phụ thuộc nhà cung cấp, khó tùy biến nghiệp vụ.
2. **Tự vận hành server truyền thống (VPS/on-premise)** — chủ động hơn nhưng tốn chi phí duy trì 24/7 dù lượng người dùng thực tế rất thấp, đồng thời phát sinh gánh nặng vận hành (patch OS, scaling, load balancer, backup thủ công).

Dự án **Smart Notes API** giải quyết bài toán này bằng kiến trúc **Serverless hoàn toàn** trên AWS:

- Không có máy chủ nào chạy thường trực → **chi phí chỉ phát sinh khi có request thực tế**.
- **Tự động mở rộng (auto-scaling)** theo lượng truy cập mà không cần cấu hình thêm.
- **Toàn quyền kiểm soát dữ liệu** (DynamoDB + S3 riêng trong tài khoản AWS của người triển khai), không phụ thuộc nhà cung cấp SaaS thứ ba.
- Áp dụng **Clean Architecture** (Controller → Service → Repository) ngay trong tầng Lambda, giúp code dễ kiểm thử (unit test), dễ bảo trì và dễ mở rộng nghiệp vụ về sau.

Ngoài chức năng CRUD ghi chú cơ bản, hệ thống còn tích hợp các yếu tố thường bị bỏ qua ở các đồ án tương tự nhưng **bắt buộc phải có trong một hệ thống sản xuất thực tế**: xác thực người dùng (Cognito), chống lạm dụng API (Usage Plan/API Key), sao lưu dữ liệu (PITR), giám sát lỗi (CloudWatch) và tự động hóa triển khai (CI/CD).

---

# 2. Vấn đề cần giải quyết

## 2.1. Vấn đề là gì?

Khi xây dựng một API quản lý ghi chú theo cách truyền thống (server chạy liên tục), hệ thống thường gặp các hạn chế sau:

| Hạn chế | Hệ quả |
|---|---|
| Server chạy 24/7 dù phần lớn thời gian không có request | Lãng phí chi phí hạ tầng, đặc biệt với ứng dụng quy mô nhỏ/cá nhân |
| Khó tự động mở rộng khi lượng người dùng tăng đột biến | Nghẽn hệ thống, trải nghiệm người dùng kém, cần can thiệp thủ công |
| Quản lý hạ tầng (patch OS, scaling, load balancer) tốn nhiều công sức | Tốn thời gian vận hành thay vì tập trung phát triển tính năng |
| API mở công khai không có lớp bảo vệ | Dễ bị quét tự động, lạm dụng, tấn công brute-force hoặc DDoS ở tầng ứng dụng |
| Không có cơ chế xác thực người dùng | Bất kỳ ai biết URL cũng có thể gọi API, rủi ro rò rỉ/thao túng dữ liệu |
| Không có sao lưu dữ liệu tự động | Rủi ro mất dữ liệu vĩnh viễn khi thao tác xóa/sửa nhầm |
| Triển khai thủ công | Dễ sai sót, khó tái lập môi trường, khó rollback khi có lỗi |

## 2.2. Giải pháp đề xuất

Smart Notes API giải quyết các vấn đề trên bằng một kiến trúc serverless đồng bộ, trong đó mỗi thành phần AWS đảm nhiệm đúng một vai trò:

- **Amazon API Gateway** tiếp nhận toàn bộ request REST (GET/POST/PUT/DELETE), đóng vai trò cổng vào duy nhất của hệ thống.
- **AWS Lambda** (Node.js 22, Express + serverless-http) xử lý nghiệp vụ theo mô hình Clean Architecture, chỉ chạy khi có request và tự hủy sau khi xử lý xong.
- **Amazon DynamoDB** lưu trữ dữ liệu ghi chú theo chế độ **PAY_PER_REQUEST**, không cần dự trù capacity trước.
- **Amazon S3** lưu trữ hình ảnh đính kèm ghi chú, có cơ chế retry khi upload lỗi.
- **Amazon API Gateway Usage Plan + API Key** giới hạn tốc độ gọi API (throttle/quota) để chống lạm dụng.
- **Amazon Cognito** (Hosted UI + liên kết Google) bắt buộc đăng nhập trước khi truy cập ghi chú, đảm bảo dữ liệu của mỗi người dùng được cách ly.
- **Giao diện web tĩnh** (HTML/CSS/JS thuần) gọi trực tiếp API qua fetch/CORS, không cần framework frontend, giảm độ phức tạp và chi phí bảo trì.

## 2.3. Lợi ích và hiệu quả đầu tư (ROI)

| Tiêu chí | Cách tiếp cận truyền thống | Smart Notes API (Serverless) |
|---|---|---|
| Chi phí khi không có traffic | Vẫn tốn phí server 24/7 | Gần như bằng 0 (Free Tier bao phủ phần lớn dịch vụ) |
| Vận hành hạ tầng | Cần tự vá lỗi, nâng cấp, giám sát máy chủ | Không cần quản lý máy chủ |
| Khả năng mở rộng | Cần cấu hình auto-scaling/load balancer thủ công | Tự động mở rộng theo lượng request |
| Bảo mật | Tự xây dựng từ đầu | IAM Least Privilege sẵn có: mỗi Lambda chỉ có đúng quyền cần thiết lên DynamoDB/S3 |
| Khả năng tái sử dụng | Gắn chặt với hạ tầng cụ thể | Kiến trúc có thể tái sử dụng cho các API CRUD khác (to-do list, bookmark, journal...) |

---

# 3. Kiến trúc giải pháp

Kiến trúc được xây dựng theo mô hình **Serverless** kết hợp **Clean Architecture** ở tầng ứng dụng.

## 3.1. Sơ đồ luồng hoạt động

```
 Client (Web tĩnh: HTML/CSS/JS)
          │
          │  fetch/CORS + x-api-key + Authorization (Cognito ID Token)
          ▼
   Amazon API Gateway (REST API)
          │
          │  Cognito Authorizer (bắt buộc đăng nhập)
          │  + Usage Plan / API Key (throttle & quota)
          ▼
   AWS Lambda (Node.js 22, Express + serverless-http)
          │
          │  Controller → Service → Repository (Clean Architecture)
          │
   ┌──────┴───────┐
   ▼              ▼
Amazon          Amazon S3
DynamoDB        (ảnh ghi chú, có retry khi upload lỗi)
(bảng Notes,
PAY_PER_REQUEST,
PITR bật)

          ▲
          │  Xác thực người dùng
   Amazon Cognito
   (User Pool + Hosted UI + Google Identity Provider)

          │
          ▼
   Amazon CloudWatch (Logs & giám sát lỗi)
```

## 3.2. Các dịch vụ AWS sử dụng

| Dịch vụ | Vai trò |
|---|---|
| **Amazon API Gateway** | Cung cấp REST API, áp Usage Plan/API Key và Cognito Authorizer |
| **AWS Lambda** | Xử lý nghiệp vụ Serverless (Node.js 22, Express + serverless-http) |
| **Amazon DynamoDB** | Lưu trữ dữ liệu ghi chú (chế độ PAY_PER_REQUEST) |
| **Amazon S3** | Lưu trữ hình ảnh đính kèm ghi chú |
| **Amazon Cognito** | Xác thực người dùng (đăng ký/đăng nhập email + Google), Hosted UI |
| **Amazon CloudWatch** | Giám sát và ghi log hệ thống, tra cứu lỗi |
| **IAM** | Quản lý quyền truy cập theo nguyên tắc Least Privilege |
| **AWS SAM (CloudFormation)** | Infrastructure as Code — đóng gói và triển khai toàn bộ hạ tầng |
| **DynamoDB Point-in-Time Recovery (PITR)** | Tự động sao lưu liên tục, khôi phục bảng Notes về bất kỳ thời điểm nào trong 35 ngày gần nhất |
| **CI/CD Pipeline** | Tự động hóa build & deploy (`sam build && sam deploy`) mỗi khi có thay đổi code |

## 3.3. Thiết kế các thành phần

**Frontend**
- HTML/CSS/JS thuần (một file tĩnh, không cần build step).
- Phong cách "thẻ mục lục thư viện" (font Fraunces + Courier Prime, dấu "FILED").
- Gọi trực tiếp API qua fetch, xác thực bằng Cognito Hosted UI.

**Backend**
- Amazon API Gateway làm cổng vào REST API.
- AWS Lambda tổ chức theo Clean Architecture: Controller (nhận request) → Service (xử lý nghiệp vụ) → Repository (thao tác dữ liệu).

**Data Layer**
- Amazon DynamoDB: bảng Notes, khóa chính theo `userId` + `noteId` để cách ly dữ liệu giữa các người dùng.
- Amazon S3: bucket riêng lưu ảnh ghi chú, đường dẫn ảnh gắn với `noteId`.

**Authentication**
- Amazon Cognito User Pool + Hosted UI.
- Google Identity Provider (đăng nhập bằng tài khoản Google).

**Security & Throttling**
- Amazon API Gateway API Key + Usage Plan giới hạn số request/giây và số request/tháng.

**Monitoring**
- Amazon CloudWatch Logs cho từng Lambda, phục vụ tra cứu lỗi và theo dõi hiệu năng.

---

# 4. Triển khai kỹ thuật

## 4.1. Các giai đoạn triển khai

| Giai đoạn | Nội dung | Trạng thái |
|---|---|---|
| 1 | Thiết kế kiến trúc & định nghĩa API (endpoints, response format, validation) | Hoàn thành |
| 2 | Triển khai IAM, Amazon DynamoDB và Amazon S3 (least-privilege) | Hoàn thành |
| 3 | Xây dựng Lambda theo Clean Architecture (Controller/Service/Repository) + serverless-http | Hoàn thành |
| 4 | Triển khai Amazon API Gateway, kiểm thử CRUD + upload/xóa ảnh qua Postman | Hoàn thành |
| 5 | Xây dựng Frontend HTML tĩnh, kết nối API qua fetch/CORS | Hoàn thành |
| 6 | Bổ sung bảo mật API Key + Usage Plan (throttle/quota) | Hoàn thành |
| 7 | Tích hợp Amazon Cognito (Hosted UI + Google Identity Provider), bắt buộc đăng nhập | Hoàn thành |
| 8 | Thiết lập giám sát CloudWatch Logs, hướng dẫn tra cứu lỗi | Hoàn thành |
| 9 | Bật Point-in-Time Recovery (PITR) cho DynamoDB, bảo vệ dữ liệu khỏi thao tác xóa/sửa nhầm | Hoàn thành |
| 10 | Thiết lập CI/CD tự động triển khai (build & deploy tự động khi push code) | Hoàn thành |
| 11 | Kiểm thử toàn diện hệ thống (CRUD, upload ảnh, đăng nhập/đăng xuất, bảo mật, khôi phục PITR) | Hoàn thành |

## 4.2. Yêu cầu kỹ thuật

- Tài khoản AWS (AWS Account) và người dùng IAM (AWS IAM User) với quyền phù hợp.
- Khu vực triển khai: **ap-southeast-1**.
- AWS CLI và AWS SAM CLI đã cấu hình.
- Node.js phiên bản 22.
- Google Cloud Console — tạo OAuth Client phục vụ liên kết đăng nhập Google qua Cognito.
- Postman — phục vụ kiểm thử API thủ công.
- Git — quản lý mã nguồn và kích hoạt CI/CD.

## 4.3. Chiến lược kiểm thử

- **Kiểm thử chức năng**: CRUD ghi chú, upload/xóa ảnh, đăng nhập/đăng xuất qua Postman và giao diện web.
- **Kiểm thử bảo mật**: xác nhận API từ chối request thiếu API Key hoặc thiếu token Cognito hợp lệ.
- **Kiểm thử khôi phục dữ liệu**: mô phỏng thao tác xóa nhầm và khôi phục bảng Notes bằng PITR.
- **Kiểm thử tải (khuyến nghị bổ sung)**: dùng công cụ như Artillery hoặc k6 để xác nhận hành vi auto-scaling của Lambda/API Gateway dưới tải cao.

---

# 5. Thời gian & Mốc quan trọng

| Trạng thái | Công việc |
|---|---|
| ✅ Đã hoàn thành | Thiết kế kiến trúc, xây dựng CRUD + upload ảnh, deploy thành công |
| ✅ Đã hoàn thành | Xây dựng frontend HTML tĩnh (edit note, xem chi tiết note) |
| ✅ Đã hoàn thành | Bảo mật API Key + Usage Plan |
| ✅ Đã hoàn thành | Tích hợp Cognito Hosted UI + đăng nhập Google |
| ✅ Đã hoàn thành | Bật Point-in-Time Recovery (PITR) cho DynamoDB |
| ✅ Đã hoàn thành | Thiết lập CI/CD tự động deploy |
| ✅ Đã hoàn thành | Hướng dẫn xem log CloudWatch khi lỗi |
| ✅ Đã hoàn thành | Kiểm thử tải và đánh giá hiệu năng dưới lượng truy cập cao |
| ✅ Đã hoàn thành | Phân trang/tìm kiếm phía backend, gắn tag, ghim note |

---

# 6. Dự toán chi phí

Có thể sử dụng **AWS Free Tier** trong quá trình phát triển và thử nghiệm. Tham khảo công cụ ước tính chính thức tại: https://calculator.aws

## 6.1. Chi phí hạ tầng ước tính (quy mô thử nghiệm/cá nhân)

| Dịch vụ | Chi phí/tháng (ước tính) | Ghi chú |
|---|---|---|
| Amazon API Gateway | Free Tier | 1 triệu request đầu tiên miễn phí (12 tháng đầu) |
| AWS Lambda | Free Tier | 1 triệu lượt gọi + 400.000 GB-giây miễn phí mỗi tháng |
| Amazon DynamoDB | Free Tier | PAY_PER_REQUEST, chi phí phát sinh tối thiểu ở quy mô nhỏ |
| Amazon S3 | ~1–2 USD | Tùy dung lượng và số lượt truy xuất ảnh |
| Amazon Cognito | Free Tier | Miễn phí cho dưới 50.000 MAU (Monthly Active Users) |
| Amazon CloudWatch | ~1–2 USD | Tùy khối lượng log |

**Tổng chi phí thử nghiệm ước tính: khoảng 2–5 USD/tháng.**

## 6.2. Lưu ý về chi phí khi mở rộng quy mô

Khi vượt ngưỡng Free Tier (ví dụ: vượt 50.000 MAU trên Cognito, hoặc lượng request/Lambda tăng cao), chi phí sẽ tăng theo mô hình pay-as-you-go của từng dịch vụ. Khuyến nghị thiết lập **Billing Alarm** (đã liệt kê ở mục 7.2) để chủ động theo dõi và kiểm soát chi phí khi hệ thống có lượng người dùng thực tế lớn hơn giai đoạn thử nghiệm.

---

# 7. Đánh giá rủi ro

## 7.1. Ma trận rủi ro

| Rủi ro | Khả năng xảy ra | Mức độ ảnh hưởng | Trạng thái xử lý |
|---|---|---|---|
| API bị lạm dụng/quét tự động khi chưa có bảo mật | Trung bình | Cao | Đã giảm thiểu nhờ API Key + Cognito Authorizer |
| Quên xóa tài nguyên AWS sau khi ngừng dùng, phát sinh phí ngoài dự kiến | Trung bình | Trung bình | Cần thiết lập Billing Alarm |
| Sai cấu hình CORS/redirect URI khi tích hợp Cognito + Google | Trung bình | Trung bình | Kiểm thử kỹ trước khi go-live |
| Mất dữ liệu do thao tác xóa/sửa nhầm | Thấp | Cao | Đã giảm thiểu nhờ bật PITR trên DynamoDB |
| Quá tải API khi nhiều người dùng truy cập cùng lúc | Thấp | Trung bình | Auto-scaling của Lambda/API Gateway; cần kiểm thử tải để xác nhận |
| Rò rỉ khóa/thông tin nhạy cảm trong mã nguồn hoặc log | Thấp | Cao | Cần rà soát định kỳ, không hardcode secrets |

## 7.2. Chiến lược giảm thiểu

- Sử dụng **Amazon CloudWatch** để giám sát lỗi và log theo thời gian thực.
- Áp dụng **IAM Least Privilege** cho từng Lambda, chỉ cấp đúng quyền cần thiết.
- Giới hạn truy cập API bằng **API Key + Usage Plan + Cognito Authorizer**.
- Thiết lập **Billing Alarm** trên AWS Budgets để theo dõi và cảnh báo chi phí phát sinh.
- Bật **Point-in-Time Recovery (PITR)** trên DynamoDB để khôi phục dữ liệu khi cần.
- Triển khai qua **CI/CD tự động**, hạn chế lỗi do thao tác deploy thủ công.
- Có kế hoạch xóa toàn bộ tài nguyên (`sam delete`) khi ngừng sử dụng để tránh phát sinh chi phí không cần thiết.

## 7.3. Phương án dự phòng

- Cơ chế **retry** khi upload ảnh lên S3 thất bại (đã tích hợp sẵn trong tầng Service).
- Lưu log lỗi trên CloudWatch để tra cứu và xử lý sự cố nhanh chóng.
- Có thể khôi phục toàn bộ hạ tầng bất kỳ lúc nào từ `template.yaml` (Infrastructure as Code), đảm bảo khả năng tái lập môi trường (disaster recovery) nhanh chóng.

---

# 8. Kết quả kỳ vọng

## 8.1. Cải tiến kỹ thuật

- Xây dựng hoàn chỉnh REST API Serverless theo Clean Architecture, dễ kiểm thử và mở rộng.
- Bảo mật nhiều lớp: API Key/Usage Plan ở tầng gateway + Cognito Authorizer ở tầng xác thực người dùng.
- Đăng nhập linh hoạt: email/mật khẩu hoặc tài khoản Google.
- Toàn bộ hạ tầng được quản lý bằng **Infrastructure as Code** (AWS SAM), đảm bảo khả năng tái lập và version control cho hạ tầng.
- Khả năng giám sát và tra cứu lỗi chủ động qua CloudWatch.
- Dữ liệu được bảo vệ bằng **Point-in-Time Recovery**, khôi phục được trong 35 ngày gần nhất.
- Quy trình triển khai tự động hóa qua **CI/CD**, giảm sai sót do deploy thủ công.

## 8.2. Giá trị lâu dài

Kiến trúc Smart Notes API được thiết kế để tái sử dụng, có thể mở rộng cho nhiều bài toán CRUD tương tự:

- Ứng dụng to-do list / quản lý công việc cá nhân.
- Ứng dụng bookmark / lưu trữ liên kết.
- Nhật ký cá nhân (journal) có đính kèm ảnh.
- Nền tảng ghi chú nhóm (team notes) nếu mở rộng thêm cơ chế phân quyền theo người dùng/nhóm.
- Bất kỳ API CRUD quy mô nhỏ nào cần triển khai nhanh, chi phí thấp trên AWS.

Nhìn chung, đây là một giải pháp **Serverless gọn nhẹ, tối ưu chi phí, dễ bảo trì**, đáp ứng các tiêu chuẩn kiến trúc cơ bản của AWS (bảo mật, khả năng phục hồi, khả năng mở rộng, tự động hóa vận hành) cho một ứng dụng quy mô cá nhân/nhỏ, đồng thời có nền tảng vững chắc để phát triển thành sản phẩm quy mô lớn hơn trong tương lai.