---
title : "Thêm dịch vụ Amazon Cognito"
date : "2025-06-14" 
weight : 3 
chapter : false
pre : " <b> 2.3 </b> "
---
Sau khi đã hiểu rõ giao diện và logic đăng nhập mô phỏng của ứng dụng MUSSEL, chúng ta sẽ bắt đầu tích hợp **xác thực người dùng thật** bằng **Amazon Cognito**, thông qua **AWS Amplify**.

---

### Mục tiêu

- Thêm hệ thống đăng nhập thực (Amazon Cognito) vào ứng dụng MUSSEL.
- Sử dụng công cụ dòng lệnh **Amplify CLI** để tạo, cấu hình và deploy backend.
- Đồng bộ backend về máy local để chuẩn bị tích hợp vào frontend.

---

### Các bước thực hiện

#### 1. Khởi tạo Amplify Project

Mở terminal trong thư mục `my-event-planner-sample-app` và chạy:

```bash
amplify init
```
Chọn các cấu hình sau:
| Câu hỏi                       | Trả lời                |
| ----------------------------- | ---------------------- |
| Name                          | eventplanner           |
| Environment                   | dev                    |
| Default editor                | Visual Studio Code     |
| App type                      | javascript             |
| JavaScript framework          | vue                    |
| Source Directory Path         | src                    |
| Distribution Directory Path   | dist                   |
| Build Command                 | npm run-script build   |
| Start Command                 | npm run-script serve   |
| Initialize project config     | Yes                    |
| Authentication method         | AWS access keys        |
| accessKeyId / secretAccessKey | (nhập từ AWS IAM user) |

Sau vài phút, bạn sẽ thấy:
```plantext
✔ Initialized your environment successfully.
✔ Your project has been successfully initialized and connected to the cloud!
```
#### 2. Thêm dịch vụ xác thực (Cognito)
Chạy lệnh: `amplify add auth`
Chọn các tùy chọn sau:
| Câu hỏi                       | Trả lời                |    
| ----------------------------- | ---------------------- |
| Do you want to use the default authentication and security configuration? | Yes |
| How do you want users to be able to sign in? | Username |
| Do you want to configure advanced settings? | No |    
  
Sau khi hoàn thành, bạn sẽ thấy:
```plaintext
✔ Successfully added auth resource eventplannerAuth
```   
#### 3. Deploy backend lên AWS    
Chạy lệnh: `amplify push`
Chọn `Yes` để xác nhận việc tạo các tài nguyên trên AWS. Quá trình này sẽ mất khoảng 1-2 phút.
Bạn sẽ thấy thông báo:
```plaintext
✔ All resources are updated in the cloud.
```
#### 4. Đồng bộ backend về local
Chạy lệnh: `amplify pull`
Chọn các tùy chọn sau:
| Câu hỏi                       | Trả lời                |  
| ----------------------------- | ---------------------- |
| Do you want to use an existing environment? | Yes |
| Select the environment you want to use | dev |
| Do you want to generate code for your newly created GraphQL API? | Yes |  

Sau khi hoàn thành, bạn sẽ thấy:
```plaintext
✔ Successfully pulled backend environment dev from the cloud.
```
#### 5. Cài đặt Amplify Libraries
Chạy lệnh sau để cài đặt các thư viện cần thiết:
```bash
npm install aws-amplify @aws-amplify/ui-vue
```
#### 6. Cập nhật cấu hình Amplify trong ứng dụng
Mở file `src/main.js` và thêm đoạn mã sau để cấu hình Amplify:

```javascript
import Amplify from 'aws-amplify';
import awsExports from './aws-exports';
Amplify.configure(awsExports);
```
#### 7. Chạy lại ứng dụng
Chạy lệnh sau để khởi động lại ứng dụng:
```bash
npm run dev
```  

Mở trình duyệt và truy cập địa chỉ `http://localhost:3000/` để kiểm tra.
Bạn sẽ thấy giao diện đăng nhập đã được cập nhật với tính năng đăng nhập thực. Bây giờ, bạn có thể đăng nhập bằng tên người dùng đã tạo trong Amazon Cognito.

---
### Kết quả
Bạn đã hoàn tất việc tích hợp **Amazon Cognito** vào ứng dụng MUSSEL. Bây giờ, người dùng có thể đăng nhập và sử dụng các tính năng của ứng dụng với xác thực thực tế.
Trong các bước tiếp theo, bạn sẽ tiếp tục xây dựng backend thực tế với **Amazon DynamoDB** và **GraphQL API**, kết nối chúng với frontend để hoàn thiện ứng dụng.   
{{% notice tip %}}
Hãy đảm bảo kiểm tra kỹ các bước và cấu hình để tránh lỗi khi deploy.
{{% /notice %}} 

### Kết luận
Bạn đã hoàn thành việc thêm dịch vụ xác thực người dùng vào ứng dụng MUSSEL bằng **Amazon Cognito**.  
Trong các bước tiếp theo, bạn sẽ tiếp tục thêm những đoạn mã để kết nối ứng dụng với backend thực tế để tạo đăng nhập và quản lý dữ liệu người dùng.
Hãy đảm bảo kiểm tra kỹ các bước và cấu hình để tránh lỗi khi deploy.

{{% notice tip %}}
Hãy đảm bảo xóa ứng dụng Amplify và toàn bộ tài nguyên sau khi hoàn thành để tránh phát sinh chi phí không cần thiết.
{{% /notice %}}

{{% notice warning %}}
Nếu bạn gặp lỗi trong quá trình cấu hình hoặc deploy, hãy kiểm tra lại các bước đã thực hiện và đảm bảo rằng bạn đã cài đặt đúng các thư viện cần thiết.  
Nếu cần, bạn có thể tham khảo tài liệu chính thức của [AWS Amplify](https://docs.amplify.aws/) để biết thêm chi tiết về các lệnh và cấu hình.
{{% /notice %}}
