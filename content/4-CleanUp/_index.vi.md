---
title : "Kết luận và Xóa tài nguyên"
date : "2025-06-14"
weight : 4 
chapter : false
pre : " <b> 4. </b> "
---
Sau khi hoàn thành Workshop **"Xây dựng Ứng dụng Web Đặt Vé Sự Kiện với AWS Amplify"**, bạn đã nắm được cách xây dựng một ứng dụng full-stack sử dụng mô hình **serverless**, từ bước xác thực người dùng bằng **Amazon Cognito** đến lưu trữ dữ liệu bằng **Amazon DynamoDB**.

---

### Tổng kết nội dung học được

- Cài đặt và cấu hình ứng dụng frontend sử dụng Vue.js
- Thêm dịch vụ backend xác thực bằng Cognito với Amplify CLI
- Thiết kế schema dữ liệu với GraphQL
- Tự động sinh bảng dữ liệu trên DynamoDB
- Kết nối frontend với backend thực qua GraphQL API
- Ghi nhận và đồng bộ vé đặt sự kiện từ người dùng

Serverless cho phép bạn xây dựng ứng dụng **linh hoạt – tiết kiệm chi phí – dễ mở rộng**, chỉ trả tiền theo nhu cầu thực tế.

---

### Dọn dẹp tài nguyên không sử dụng

Nếu bạn sử dụng tài khoản AWS cá nhân, **bạn nên xóa toàn bộ tài nguyên sau khi workshop kết thúc** để tránh bị tính phí duy trì không cần thiết.  
Để xóa tài nguyên backend đã tạo trong quá trình workshop, bạn có thể sử dụng lệnh Amplify CLI.  
Đảm bảo bạn đã cài đặt Amplify CLI và đã đăng nhập vào tài khoản AWS của mình.  
Trong thư mục gốc của project, chạy:

```bash
amplify delete
```
Quá trình này sẽ:  
- Xóa toàn bộ tài nguyên backend trên AWS (Cognito, DynamoDB, AppSync,...)  
- Xóa luôn file cấu hình Amplify local như schema.graphql, amplify/,...  
{{% notice warning %}}
Nếu muốn giữ schema hay config, hãy sao lưu trước khi xóa.
{{% /notice %}}
- Quá trình xóa sẽ mất một chút thời gian, tùy thuộc vào số lượng tài nguyên đã tạo. Bạn sẽ được hỏi xác nhận trước khi xóa, hãy gõ `Y` để tiếp tục.  
- Khi xóa thành công, bạn sẽ thấy thông báo:
```bash
√ Project deleted in the cloud.
✅ Project deleted locally.
``` 
![DB](/images/3.connect/03-DB3.png)
{{% notice warning %}}
Nếu bạn KHÔNG xóa ứng dụng Amplify, tài nguyên như DynamoDB hay Cognito vẫn tiếp tục chạy trên tài khoản AWS và phát sinh chi phí!
{{% /notice %}}

### ✅ Hoàn thành workshop!
Chúc mừng bạn đã hoàn tất toàn bộ nội dung.  
Hy vọng bạn sẽ tiếp tục khám phá các giải pháp serverless khác từ AWS như:  
- Amazon S3 (lưu ảnh sự kiện)  
- AWS Lambda (xử lý logic phía backend)  
- Amazon API Gateway (REST API)  
- Amazon AppSync nâng cao  

**Hẹn gặp lại bạn trong các workshop tiếp theo! Chúc bạn thành công!**  
