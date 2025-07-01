---
title: "Serverless Image Processing với S3 Events, Rekognition và Step Functions"
date: "2025-06-14"
weight: 1
chapter: false
---

# 📌 Thông tin Workshop

**Tên Workshop:** Serverless Image Processing với S3 Events, Rekognition và Step Functions  
**Thực hiện bởi:** Diệp Hoài An  
**MSSV:** 21110001  
**Email:** diephoaian2003@gmail.com  
**Trường:** Đại học Sư phạm Kỹ thuật TP.HCM (HCMUTE)  
**Chương trình:** AWS First Cloud Journey Internship  
**Ngày hoàn thành:** 04/07/2025  

---

# Xử lý ảnh bằng Serverless với S3 Events, Rekognition và Step Functions 

## Giới thiệu   
Workshop này hướng dẫn cách triển khai một pipeline xử lý ảnh đại diện người dùng hoàn toàn bằng mô hình **serverless** trên AWS. Hệ thống thực hiện:
- **Phát hiện khuôn mặt**
- **Kiểm tra ảnh trùng lặp**
- **Tạo ảnh thu nhỏ**
- **Xác thực định dạng ảnh**
- **Trích xuất và lưu metadata**
- **Tự động xoá ảnh lỗi**
- **Gửi kết quả thành công/thất bại**

Các dịch vụ được sử dụng gồm:

- **Amazon Rekognition** để phân tích và đối chiếu khuôn mặt  
- **AWS Lambda** để xử lý từng bước ảnh  
- **Step Functions** để điều phối toàn bộ quy trình xử lý  
- **Amazon S3** để lưu trữ ảnh gốc/thumbnail và làm website tĩnh  
- **DynamoDB**, **SNS**, **EventBridge** để lưu thông tin, gửi thông báo và kích hoạt xử lý  

👉 Kết quả xử lý cuối cùng được hiển thị trực tiếp qua một **website tĩnh trên S3**.

---

## Yêu cầu & Kỹ năng đạt được

Workshop này được thiết kế để giúp bạn thành thạo:

- Phát hiện khuôn mặt qua Rekognition
- Kiểm tra trùng lặp khuôn mặt đã có trong hệ thống
- Resize ảnh để tạo thumbnail
- Kiểm tra định dạng hợp lệ (.jpg, .png)
- Trích xuất metadata và lưu vào DynamoDB
- Tự động dọn dẹp ảnh không hợp lệ hoặc lỗi
- Gửi thông báo thành công/thất bại bằng SNS
- Thiết lập workflow serverless với Step Functions
- Tối ưu hoá chi phí và cấu hình theo dõi qua AWS console
- Upload và hiển thị kết quả qua giao diện website S3
- Mô tả, phân tích và đánh giá quy trình vận hành

---

## Thời gian thực hiện

1.5 – 2.5 tiếng  
(Phụ thuộc vào mức độ quen thuộc của bạn với AWS)

---

## Nội dung Workshop

1. [Giới thiệu](1-introduce/)
2. [Chuẩn bị môi trường (VPC, EC2, IAM)](2-prerequiste/)
3. [Tạo kết nối đến EC2 & upload ảnh](3-accessibilitytoinstances/)
4. [Quản lý session logs với S3](4-s3log/)
5. [Port Forwarding & giao diện S3 Website](5-portfwd/)
6. [Dọn dẹp tài nguyên & tổng kết](6-cleanup/)
