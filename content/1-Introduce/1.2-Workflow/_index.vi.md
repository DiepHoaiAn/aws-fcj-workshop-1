---
title : "Quy trình xử lý ảnh"
date : "2025-06-14"
weight : 1 
chapter : false
pre : " <b> 1.2 </b> "
---

Phần này giúp bạn hình dung rõ ràng toàn bộ quy trình hệ thống sẽ thực hiện khi một người dùng **tải ảnh đại diện** lên. Đây là bước tổng quan cần thiết trước khi ta triển khai từng phần cụ thể trong các chương sau.

---

## Mục tiêu của hệ thống

- Đảm bảo mỗi người dùng chỉ được dùng **1 ảnh hợp lệ**, không trùng.
- Tự động kiểm tra nội dung ảnh và xác nhận kết quả.
- Tối ưu tài nguyên lưu trữ, không giữ ảnh không cần thiết.
- Phản hồi nhanh và rõ ràng cho người dùng qua email.

## Dịch vụ được sử dụng

| Dịch vụ AWS         | Mục đích sử dụng                                                 |
|---------------------|------------------------------------------------------------------|
| **Amazon S3**       | Lưu trữ ảnh upload và ảnh đã xử lý                              |
| **Amazon Rekognition** | Phát hiện và đối chiếu khuôn mặt                               |
| **AWS Lambda**      | Xử lý ảnh, resize, kiểm tra nội dung                            |
| **AWS Step Functions** | Điều phối toàn bộ quy trình xử lý                             |
| **Amazon SNS**      | Gửi email xác nhận hoặc lỗi                                      |
| **Amazon DynamoDB** | Lưu metadata ảnh đã xử lý                                        |

## Giai đoạn xử lý ảnh

#### 1. Người dùng tải ảnh lên

- Giao diện web (host trên S3 Static Website) cho phép chọn hoặc kéo-thả ảnh.
- Ảnh được upload trực tiếp lên một bucket riêng, chỉ chấp nhận .jpg hoặc .png.
- Không cần login, xác thực bằng Cognito Identity Pool.

📌 *Đây là điểm bắt đầu cho toàn bộ hệ thống.*

#### 2. Bắt đầu xử lý ảnh

- Khi ảnh mới được lưu, S3 kích hoạt sự kiện ObjectCreated.
- Sự kiện này đi qua Amazon EventBridge, kích hoạt Step Function để xử lý tự động.

#### 3. Xử lý ảnh theo luồng

Step Function điều phối các bước theo trình tự:

**1. Kiểm tra định dạng**
   - Ảnh phải đúng định dạng: .jpg, .png.
   - Nếu sai → không thể upload file.  

**2. Phát hiện khuôn mặt**
   - Gọi Rekognition để kiểm tra có khuôn mặt người hay không.
   - Nếu không thấy mặt → gửi email lỗi.  

**3. Kiểm tra trùng lặp**
   - Đối chiếu ảnh với các ảnh đã lưu trong collection Rekognition.
   - Nếu có trùng (match trên 95%) → thông báo lỗi.  

**4. Xử lý ảnh hợp lệ** (chạy song song):
   - Resize ảnh → tạo thumbnail.
   - Index khuôn mặt → lưu vào Rekognition Collection.

#### 4. Lưu thông tin và dọn dẹp

- Ghi metadata (ngày upload, tên file, user ID...) vào DynamoDB.
- Xoá ảnh gốc khỏi bucket S3 để tiết kiệm dung lượng.

#### 5. Phản hồi cho người dùng

- Nếu ảnh hợp lệ → gửi email xác nhận thành công.
- Nếu ảnh lỗi (sai định dạng, không có mặt, trùng lặp) → gửi email báo lỗi.

## Sơ đồ minh họa tổng thể

{{< figure src="/images/1.introduction/State_machine_flow1.png" title="Luồng xử lý ảnh bằng Step Functions" >}}

> *Sơ đồ thể hiện rõ các bước xử lý từ lúc ảnh được upload đến khi phản hồi kết quả – minh hoạ quá trình xử lý tự động không cần người kiểm duyệt.*

## Lợi ích của hệ thống

- **Tự động hoá 100%**: không cần backend, không cần nhân sự kiểm duyệt.
- **Giảm chi phí lưu trữ**: chỉ giữ lại ảnh hợp lệ, ảnh gốc bị xoá.
- **Trải nghiệm người dùng tốt**: nhận phản hồi ngay qua email.
- **Ứng dụng thực tế cao**: có thể triển khai cho eKYC, tuyển sinh, sự kiện,...

---

> Bắt đầu từ phần 3.1, ta sẽ lần lượt triển khai từng phần trong hệ thống trên: từ giao diện upload, đến các hàm xử lý ảnh, kết nối dịch vụ AWS, và hoàn thiện toàn bộ pipeline serverless.
