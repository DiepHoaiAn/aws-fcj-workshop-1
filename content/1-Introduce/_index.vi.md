---
title: "Giới thiệu"
date: "2025-06-14"
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---
### Tổng quan

Chương này sẽ giúp bạn có cái nhìn toàn diện về mục tiêu, yêu cầu kỹ năng và tình huống thực tế được mô phỏng trong suốt workshop **"Xây dựng Ứng dụng Web Đặt Vé Sự Kiện với AWS Amplify"**. Đây là phần mở đầu quan trọng giúp bạn nắm rõ định hướng, công nghệ cốt lõi và những gì bạn sẽ đạt được sau khi hoàn thành toàn bộ các bước.

Workshop này được thiết kế không chỉ để rèn luyện kỹ thuật sử dụng **AWS Amplify**, mà còn để hiểu được cách xây dựng một ứng dụng thực tế – có xác thực người dùng, truy xuất dữ liệu từ backend, và lưu thông tin trên cơ sở dữ liệu cloud-native bằng GraphQL.

---

### Từ bài toán thực tế đến giải pháp trên AWS

Trong nhiều tổ chức, đặc biệt là các trường đại học hoặc đơn vị tổ chức sự kiện, nhu cầu quản lý sự kiện và vé thường diễn ra thủ công hoặc qua file Excel, khó kiểm soát và mở rộng. Workshop này mô phỏng yêu cầu xây dựng một hệ thống:

- Cho phép người dùng **đăng nhập, xem sự kiện, đặt vé**.
- Dữ liệu **được lưu trữ tập trung trên cloud**, đảm bảo tính khả dụng và bảo mật.
- Giao diện thân thiện, **hoạt động hoàn toàn trên trình duyệt** (SPA – Single Page Application).

---

### Sơ đồ kiến trúc tổng quan

Để có cái nhìn trực quan về toàn bộ hệ thống mà bạn sẽ xây dựng trong workshop này, hãy cùng quan sát sơ đồ kiến trúc dưới đây.

![Kiến trúc tổng quan của ứng dụng đặt vé sự kiện](images/1.introduction/01-Architecture.png)

#### Giải thích sơ đồ:

- **User (Browser):** người dùng truy cập ứng dụng qua trình duyệt, giao tiếp qua HTTP.
- **Vue.js Web App:** frontend viết bằng Vue.js, chạy tại `localhost:3000`, chịu trách nhiệm xử lý UI, đăng nhập và gửi truy vấn dữ liệu.
- **Amplify Auth Library:** dùng để đăng nhập, xác thực người dùng với Amazon Cognito.
- **Amplify GraphQL Client:** gửi truy vấn và đột biến (mutation) tới AWS AppSync, kèm theo token xác thực nếu có.
- **Amazon Cognito:** xác thực người dùng và trả về token JWT để các dịch vụ backend xác minh quyền truy cập.
- **AWS AppSync:** dịch vụ GraphQL API, đóng vai trò trung gian giữa client và cơ sở dữ liệu DynamoDB.
- **Amazon DynamoDB:** nơi lưu trữ dữ liệu thực như bảng *Room Table* và *Event Table*.

#### Luồng hoạt động cơ bản của hệ thống:
- Người dùng mở trình duyệt và truy cập ứng dụng tại localhost:3000.  
- Ứng dụng frontend (Vue.js) hiển thị giao diện và chờ người dùng đăng nhập.  
- Khi người dùng đăng nhập, thư viện **Amplify Auth** sẽ gửi yêu cầu đến **Amazon Cognito** để xác thực.  
- Nếu đăng nhập thành công, Cognito trả về một **token xác thực (JWT)**.  
- Token này được đính kèm trong tất cả các truy vấn hoặc mutation mà **Amplify GraphQL Client** gửi đi.  
- **AppSync (GraphQL API)** nhận các truy vấn, kiểm tra token, và nếu hợp lệ thì đọc hoặc ghi dữ liệu tương ứng trong **DynamoDB**.  
- Dữ liệu trả về (ví dụ: danh sách sự kiện, thông tin đặt vé...) sẽ được gửi ngược lại frontend để hiển thị cho người dùng.  

#### Mục tiêu của kiến trúc:

- **Serverless hoàn toàn:** bạn không cần quản lý máy chủ, mọi thứ hoạt động trên dịch vụ managed của AWS.
- **Tách biệt frontend - backend:** frontend gửi truy vấn thông qua API, không truy cập trực tiếp DB.
- **Bảo mật:** chỉ người dùng đã đăng nhập (qua Cognito) mới được phép thực hiện các thao tác cập nhật dữ liệu.
- **Khả năng mở rộng cao:** hệ thống có thể mở rộng dễ dàng nếu người dùng tăng lên trong tương lai.

{{% notice info %}}
Toàn bộ quá trình diễn ra không cần backend tự xây dựng — mọi dịch vụ đều do AWS quản lý và tự động mở rộng.
{{% /notice %}}


---

### Tình huống thực tế và hướng giải quyết

| Tình huống mô phỏng | Giải pháp được áp dụng |
|----------------------|-------------------------|
| Người dùng phải đăng nhập để đặt vé | Dùng **Amazon Cognito** để xác thực và quản lý người dùng |
| Quản trị viên muốn quản lý dữ liệu trên cloud, không lưu local | Dùng **DynamoDB** để lưu các bảng `Room`, `Event` và thực hiện CRUD |
| Frontend cần tương tác nhanh, đơn giản với backend | Dùng **AppSync (GraphQL API)** để thao tác dữ liệu hiệu quả |
| Không có DevOps hoặc backend team riêng | Dùng **Amplify CLI + Hosting** để triển khai toàn bộ dịch vụ trong 1 dòng lệnh |

---

### Tại sao chọn Amplify?

- Tối ưu cho frontend dev: không cần viết backend, chỉ cần biết JS.
- Tích hợp nhanh chóng với Cognito, AppSync, DynamoDB.
- CLI thân thiện, có thể triển khai, update, xóa backend bằng 1 dòng lệnh.
- Sinh mã code tự động (GraphQL queries, mutations) để dễ sử dụng trong frontend.  

{{% notice tip %}}
Toàn bộ kiến trúc sử dụng mô hình Serverless – giúp bạn tiết kiệm chi phí, dễ triển khai và không cần lo lắng về vận hành backend.
{{% /notice %}}

---

### Kết luận

Chương 1 không chỉ giúp bạn hiểu **mục tiêu và tình huống** của bài thực hành, mà còn cho bạn cái nhìn toàn diện về **kiến trúc hạ tầng AWS** được sử dụng. Các chương tiếp theo sẽ đi vào chi tiết từng thành phần của hệ thống: từ xác thực người dùng, truy xuất dữ liệu thực, đến việc cập nhật và đồng bộ vé sự kiện.


### Nội dung tiếp theo
- [1.1 Mục tiêu học tập](./1.1-LearningObjectives/)
- [1.2 Yêu cầu kỹ thuật & kỹ năng](./1.2-Prerequisites/)
- [1.3 Tình huống mô phỏng](./1.3-Scenario/)