---
title: "Blog 3"
date: 2026-07-21
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Vận hành thực chiến: CI/CD tự động và bảo vệ dữ liệu bằng PITR

Bài blog thứ 3 trong chuỗi chia sẻ về dự án **Smart Notes API**. Nếu 2 bài trước xoay quanh xây và bảo mật hệ thống, bài này nói về phần ít được để ý nhưng rất quan trọng: **vận hành (operations)**.

## Vấn đề

Sau khi API chạy ổn và bảo mật ổn, mình nhận ra 2 lỗ hổng:

1. Mỗi lần sửa code phải tự gõ `sam build && sam deploy` trên máy — dễ quên chạy test, chỉ máy mình mới deploy được.
2. Xóa nhầm dữ liệu (thao tác sai hoặc bug xóa hàng loạt) là mất vĩnh viễn — DynamoDB không có "thùng rác cấp bảng" mặc định.

Giải quyết bằng **CI/CD** và **PITR**.

## CI/CD với GitHub Actions

AWS không có "1 dịch vụ CI/CD" duy nhất, phải tự ráp từ các công cụ. Với quy mô cá nhân, mình chọn **GitHub Actions** thay vì CodePipeline/CodeBuild — đơn giản, không tốn thêm chi phí.

Mỗi lần push lên `main`, workflow tự động chạy:

```
Checkout code → Cài dependency → Unit test (Jest)
→ Pass → sam build → sam deploy
```

Điểm quan trọng nhất: **test luôn chạy trước deploy**. Test fail thì pipeline dừng ngay, hạ tầng thật không bị đụng vào — nên push code tự tin hơn hẳn.

Chi tiết dễ vấp: 2 tham số Google Client ID/Secret (Cognito) là `NoEcho` nên `samconfig.toml` không lưu được. Trong pipeline phải truyền lại qua `--parameter-overrides`, lấy từ GitHub Secrets — không hardcode vào repo.

## Point-in-Time Recovery (PITR) cho DynamoDB

Tính năng "lời" nhất so với công sức bỏ ra — chỉ 2 dòng trong `template.yaml`:

```yaml
NotesTable:
  Properties:
    PointInTimeRecoverySpecification:
      PointInTimeRecoveryEnabled: true
```

Bật xong, DynamoDB tự sao lưu liên tục, khôi phục bảng về bất kỳ thời điểm nào trong 35 ngày gần nhất — không cần tự viết script backup hay lịch EventBridge. Chi phí gần như không đáng kể với app cá nhân.

Khôi phục thử:

```bash
aws dynamodb restore-table-to-point-in-time \
  --source-table-name Notes-dev \
  --target-table-name Notes-dev-restored \
  --restore-date-time "2026-07-20T10:00:00Z"
```

Lưu ý: nó tạo **bảng mới**, không ghi đè bảng đang chạy — muốn dùng lại dữ liệu cũ phải tự chuyển đổi thêm, không phải "undo" tức thì.

## Nhìn lại

Các điểm chính:

* CI/CD với GitHub Actions: test luôn chạy trước deploy, code lỗi không bao giờ lên production.
* Tham số nhạy cảm (`NoEcho`) truyền qua `--parameter-overrides` + GitHub Secrets, không hardcode.
* PITR trên DynamoDB: 2 dòng cấu hình, khôi phục về bất kỳ thời điểm trong 35 ngày.
* Restore PITR tạo bảng mới, không ghi đè — cần bước chuyển đổi dữ liệu thủ công.

Bài học lớn nhất đợt thực tập: hệ thống "chạy được" và hệ thống "vận hành được lâu dài" là hai chuyện khác nhau — AWS có gần như đủ công cụ, chỉ cần biết mình đang thiếu gì để tìm đúng dịch vụ.
**Xem thêm:**
* [Bài blog 1: Xây dựng Smart Notes API](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2220328232065470/?rdid=8cYdNHkX01xL0SYL#)
* [Bài blog 2: Bảo mật với API Key và Cognito](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2220307962067497/?rdid=St8Z9r4R01fFMtL7#)
* [Bài viết gốc trên Facebook (AWS Study Group FCJ)](https://www.facebook.com/groups/awsstudygroupfcj/permalink/2220307175400909/?rdid=4S6HwU2Sbma5XR0i#)