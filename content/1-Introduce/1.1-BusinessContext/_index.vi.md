---
title : "Bối cảnh"
date : "2025-06-14"
weight : 1 
chapter : false
pre : " <b> 1.1 </b> "
---

Hệ thống được thiết kế nhằm mô phỏng lại **quy trình xử lý ảnh đại diện người dùng** – một tính năng quen thuộc và thiết yếu trong rất nhiều nền tảng như:

- Mạng xã hội (Facebook, Zalo, Instagram)
- Cổng đăng ký sự kiện, hệ thống học tập trực tuyến (LMS)
- Ứng dụng eKYC hoặc quản lý tài khoản nội bộ
![Business Context](/images/1.introduction/Bussiness_Context.jpg)
---
## Mục tiêu

Hệ thống được áp dụng vào bối cảnh quản lý hồ sơ học viên tại một học viện. Mỗi học viên cần cung cấp một ảnh đại diện duy nhất, hợp lệ và không trùng với ảnh của người khác.  

Việc xử lý ảnh cần đảm bảo:
- Kiểm tra định dạng ảnh đầu vào (.jpg / .png)
- Xác định có khuôn mặt trong ảnh
- Đảm bảo ảnh không bị trùng với người đã đăng ký trước
- Tự động phản hồi qua email về trạng thái xử lý

Đây là nhu cầu phổ biến của các hệ thống:

- Quản lý tuyển sinh và sinh viên
- Đăng ký tài khoản học viên nội bộ
- Cổng thông tin e-learning hoặc các nền tảng onboarding


## Giá trị học được từ dự án

Qua quá trình triển khai hệ thống, chúng ta sẽ học được:

- Hiểu và ứng dụng **kiến trúc không máy chủ (serverless)** hiện đại
- Làm việc với các dịch vụ AWS như: S3, Lambda, Rekognition, Step Functions, SNS
- Biết cách xây dựng một pipeline xử lý bất đồng bộ theo sự kiện (event-driven)
- Đảm bảo tính tự động hóa cao, giảm thiểu thao tác thủ công
- Tối ưu chi phí lưu trữ và hiệu suất hệ thống thông qua việc xử lý song song, xoá ảnh gốc, ghi log và gửi thông báo tự động


> Phần tiếp theo sẽ trình bày về **quy trình kỹ thuật xử lý ảnh đại diện**: từ lúc người dùng tải ảnh lên, cho đến khi ảnh được phân tích, xác thực, lưu metadata và gửi phản hồi. Đây là nền tảng để người đọc có cái nhìn tổng quan về quy trình xử lý ảnh được thực hiện trong dự án này.
