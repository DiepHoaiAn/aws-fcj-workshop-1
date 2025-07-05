---
title : "Xác thực Web App với AWS Amplify"
date : "2025-06-14"
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---
### Mục tiêu

Lab này giúp bạn khởi động ứng dụng frontend mẫu MUSSEL và thiết lập tính năng xác thực (Authentication & Authorization – hay còn gọi là **AuthN** & **AuthZ**) bằng **AWS Amplify + Amazon Cognito**.

Trước khi bắt đầu, hãy chắc chắn rằng bạn đã cài đặt các công cụ theo yêu cầu ở mục **1.2 Chuẩn bị môi trường và cài đặt công cụ**.

---

#### Bước 1: Clone ứng dụng mẫu từ GitHub

1. Mở **Visual Studio Code**.

2. Chọn biểu tượng **Source Control** (Biểu tượng hình nhánh cây ở sidebar trái).

3. Click **Clone Repository**.

4. Khi thanh **Command Palette** hiện ra, dán đường dẫn sau:
https://github.com/build-on-aws/my-event-planner-sample-app.git 

5. Chọn thư mục lưu local (nên chọn thư mục dễ nhớ như `C:\code` hoặc `~/Projects`).

{{% notice tip %}}
Tránh dùng thư mục đồng bộ đám mây như Dropbox, OneDrive,... để lưu repo — có thể gây lỗi khi chạy app.
{{% /notice %}}

6. Khi được hỏi: **"Would you like to open the cloned repository?"** → chọn **Yes**.

7. Nếu xuất hiện hộp thoại **"Do you trust the authors..."**, hãy tick chọn **"Yes, I trust the authors"**.

---

#### Bước 2: Chọn đúng nhánh (branch) để làm việc

1. Nhìn ở góc dưới bên trái thanh **Status Bar**, click vào tên nhánh (thường là `main`).

2. Chọn nhánh: `origin/lab1-start` 

Khi thành công, VS Code sẽ hiển thị `lab1-start` như hình dưới:

![Chọn branch lab1-start](/images/2.prerequisite/01-branch-lab1-start.png)

---

#### Bước 3: Cài đặt dependencies

1. Mở **Command Palette** bằng phím tắt:
- **Windows**: `Ctrl + Shift + P`
- **Mac**: `Cmd + Shift + P`

2. Nhập: `> Tasks: Run Task` → nhấn `Enter`.

3. Chọn: `npm` → `npm: install`
> Quá trình này sẽ tải và cài đặt toàn bộ thư viện frontend.

---

#### Bước 4: Chạy thử ứng dụng

1. Lặp lại thao tác: `Tasks: Run Task`.

2. Chọn: `npm` → `npm: dev`

3. Terminal sẽ hiển thị như sau:
```
VITE v3.2.8 ready in 522 ms

➜ Local: http://localhost:3000/
➜ Network: http://172.31.38.233:3000/
Press h to show help
```

4. Mở trình duyệt và truy cập địa chỉ `http://localhost:3000/` để kiểm tra giao diện ứng dụng.

{{< figure src="/images/2.prerequisite/mussel-local-preview.png" >}}

---

#### Bước 5: Dừng ứng dụng

- Trong cửa sổ **Terminal**, click biểu tượng 🗑 **Kill Terminal** ở góc trên bên phải.
- Bạn có thể để terminal chạy và sửa code, trang web sẽ tự cập nhật nếu dùng hot-reload.  
- Khám phá code bằng cách click **Explorer** (biểu tượng thư mục trái trên VS Code).  
- Nếu cần, bạn có thể mở thêm terminal mới bằng cách click biểu tượng `+` ở góc trên bên phải cửa sổ terminal.
{{% notice warning %}}
Nếu không tắt đúng cách, khi bạn chạy lại `npm: dev`, sẽ có nhiều instance chạy song song, gây xung đột hoặc làm máy chạy chậm.
{{% /notice %}}

### Kết luận

Bạn đã hoàn tất việc **cài đặt và chạy ứng dụng frontend mẫu MUSSEL** ở chế độ local.  
Các bước tiếp theo sẽ giúp bạn bắt đầu tích hợp **AWS Amplify** để bổ sung backend thực tế.


