---
title : "Chuẩn bị môi trường và cài đặt công cụ"
date : "2025-06-14"
weight : 2 
chapter : false
pre : " <b> 1.2 </b> "
---
### Chuẩn bị thiết bị và tài khoản

1. Bạn cần một thiết bị chạy **Windows**, **macOS** hoặc **Linux**.  
   + Hệ điều hành không quan trọng, miễn là các phần mềm bên dưới hoạt động được.

2. Tài khoản AWS:
   + Nếu tham gia **hosted workshop**, bạn sẽ được cấp tài khoản AWS tạm thời để sử dụng.
   + Nếu học **tự túc**, bạn cần đăng ký và sử dụng tài khoản AWS cá nhân.

{{% notice warning %}}
Workshop này sẽ tạo ra các dịch vụ trên AWS có thể phát sinh chi phí.   
**Hãy đảm bảo xóa ứng dụng Amplify và toàn bộ tài nguyên sau khi hoàn thành.**
{{% /notice %}}


### Cài đặt phần mềm cần thiết

1. Cài đặt **Node.js** (kèm theo NPM):  
   + Tải tại: [https://nodejs.org/en/download/](https://nodejs.org/en/download/)

2. Cài đặt **AWS Amplify CLI** bằng lệnh:
   ```bash
   npm install -g @aws-amplify/cli
   ```
3. Cài đặt **TypeScript** (local dev):
   ```bash
   npm install -g typescript
   ```
4. Cài đặt **Git**:
+ Tải tại: [https://git-scm.com/downloads](https://git-scm.com/downloads)

5. Cài đặt **Visual Studio Code**:
+ Tải tại: [https://code.visualstudio.com/download](https://code.visualstudio.com/download)

---

### Kiểm tra phiên bản phần mềm

Sau khi cài đặt, hãy kiểm tra các phiên bản để đảm bảo đúng hoặc mới hơn:
   ```bash
   node -v
   npm -v
   amplify -v
   git -v
   code -v
   npm ls typescript
   ```
Bạn sẽ nhận được các phiên bản tương ứng tương tự như sau (hoặc mới hơn):
![Check_version](/images/1.introduction/check_version.png)

---

### Cấu hình IAM Access Keys cho Amplify CLI

1. Truy cập **AWS Console** → **IAM** → **Users** → **Create user**.

2. Đặt tên user là: `amplify-dev` (hoặc tên tùy ý).

3. Cấp quyền cho user:
   + `AdministratorAccess-Amplify`
   + `AmplifyBackendDeployFullAccess`

4. Tạo Access Key:
   + Chọn phương thức "Programmatic access"
   + Tải và lưu lại Access Key ID & Secret Access Key an toàn.


{{% notice info %}}
Trong quá trình workshop, bạn sẽ cần dùng các thông tin truy cập này khi chạy `amplify init`.  
Nếu bạn tham gia workshop do AWS tổ chức, các thông tin này có thể đã được cấp sẵn và không cần tạo mới.  
**Hãy đảm bảo không chia sẻ Access Key của bạn với người khác để tránh rủi ro bảo mật.**
{{% /notice %}}

{{% notice tip %}}
Nếu bạn không quen với việc quản lý Access Key, hãy tham khảo hướng dẫn chính thức từ AWS để hiểu rõ hơn về cách bảo mật và sử dụng.   
Nếu gặp lỗi trong quá trình cài đặt hoặc cấu hình, hãy xem tài liệu hoặc hỏi cộng đồng (ví dụ AWS study group).
{{% /notice %}}


### Tài liệu tham khảo
- [AWS Amplify CLI Documentation](https://docs.amplify.aws/cli/)
- [Node.js Documentation](https://nodejs.org/en/docs/)
- [Visual Studio Code Documentation](https://code.visualstudio.com/docs)
- [Git Documentation](https://git-scm.com/doc)
- [AWS IAM Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
