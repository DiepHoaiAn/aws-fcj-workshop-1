---
title : "Mô hình hóa dữ liệu với AWS Amplify và GraphQL"
date : "2025-06-14"
weight : 1 
chapter : false
pre : " <b> 3.1. </b> "
---
Sau khi đã cài đặt và chuẩn bị môi trường, chúng ta sẽ tạo mô hình dữ liệu (entities) và kết nối với cơ sở dữ liệu thực tế bằng **AWS Amplify** và **GraphQL**. Đây là bước đầu tiên để lưu trữ các sự kiện, phòng và vé trong **Amazon DynamoDB**.

---

### Mục tiêu

- Chuyển từ dữ liệu mô phỏng (mock) sang lưu trữ thực.
- Tạo schema GraphQL với các thực thể (entities): `Room`, `Event`, `Booking`.
- Sử dụng Amplify CLI để generate bảng DynamoDB và code tương ứng.

---
### Các bước thực hiện

#### Bước 1: Chuyển sang branch *lab2-start*
Mở terminal và chuyển đến thư mục dự án. Sau đó, checkout sang nhánh `lab2-start`:

```bash
git checkout lab2-start
```
Hoặc bạn có thể sử dụng giao diện Git trong VS Code để chuyển nhánh.
Bạn có thể tìm thấy nhánh này trong danh sách các nhánh của repo ở góc dưới bên trái của VS Code
![Change branch](/images/3.connect/01-branch.png)

{{% notice warning %}}
Nếu bạn chưa clone repo, hãy làm theo hướng dẫn trong phần **1.2 Chuẩn bị môi trường và cài đặt công cụ**.
{{% /notice %}}

{{% notice info %}}
Nhánh lab2-start chứa toàn bộ kết quả từ lab 1, sẵn sàng để tiếp tục lab 2.
{{% /notice %}}

Sau đó chạy: `npm install` để cài đặt các package cần thiết.

#### Bước 2: Thêm dịch vụ API với GraphQL
Trong thư mục gốc dự án, chạy: `amplify add api`
Chọn cấu hình sau khi được hỏi:
| Câu hỏi                                         | Trả lời                   |
| ----------------------------------------------- | ------------------------- |
| Select from one of the below mentioned services | GraphQL                   |
| Authorization modes                             | API key (default)         |
| API key description                             | Read Access               |
| Expiration (days)                               | 7                         |
| Configure additional auth types?                | Yes                       |
| Choose additional authorization types           | Amazon Cognito User Pool  |
| Use Cognito user pool configured in project?    | Yes                       |
| Choose a schema template                        | Single object with fields |
| Do you want to edit the schema now?             | Yes                       |

#### Bước 3: Viết schema cho các thực thể

Amplify sẽ mở file amplify/backend/api/.../schema.graphql. Nếu chưa mở, bạn có thể tìm theo tên.  
Xóa toàn bộ nội dung cũ (ví dụ như type Todo) và thay bằng:
```graphql
input AMPLIFY { globalAuthRule: AuthRule = { allow: public } } # FOR TESTING ONLY!

type Booking {
  datetime_start: AWSDateTime!
  datetime_end: AWSDateTime!
}

type Room @model
  @auth(rules: [{ allow: public, operations: [create, read, delete] }]) {
  id: ID!
  name: String!
  capacity: Int!
  bookings: [Booking]!
}

type Event @model
  @auth(rules: [{ allow: public, operations: [create, read, update, delete] }]) {
  id: ID!
  name: String!
  description: String!
  event_owner: String!
  room_id: String!
  image_file_name: String!
  event_datetime_start: AWSDateTime!
  event_datetime_end: AWSDateTime!
  event_duration: Int!
  total_tickets: Int!
  tickets: [String!]!
}
```
{{% notice warning %}}
Đây là lab demo nên toàn bộ dữ liệu đều công khai (public) – không dùng trong môi trường thật!
{{% /notice %}}

#### Bước 4: Deploy schema lên AWS
Sau khi lưu file **schema.graphql**, đẩy schema lên cloud: `amplify push --y`

Quá trình này có thể mất vài phút. Amplify sẽ:  
- Tạo các bảng DynamoDB tương ứng với Room và Event.  
- Sinh mã GraphQL CRUD tự động trong thư mục src/graphql.  

### Kết quả sau bước này
- Các thực thể dữ liệu đã được định nghĩa và triển khai lên DynamoDB.  
- Mã nguồn CRUD (query, mutation) đã sẵn sàng để tích hợp vào frontend.  
- Ứng dụng đã có backend thực để quản lý sự kiện và phòng.  

{{% notice tip %}}  
Bạn có thể xem các bảng đã tạo trong AWS Console tại DynamoDB → Tables.
Các bảng thường có tên bắt đầu bằng *eventplanner-<env>-Room* hoặc *...-Event*.
{{% /notice %}}

{{% notice info %}}
Các bảng DynamoDB có thể xem trong AWS Console → DynamoDB → Tables.
Tên bảng thường bắt đầu bằng eventplanner-<env>-Room hoặc ...-Event.
{{% /notice %}}

Tiếp theo: Kết nối frontend với backend thực tế thông qua các truy vấn GraphQL bước tiếp theo là **kết nối frontend với cơ sở dữ liệu thật** bằng cách sử dụng các truy vấn và đột biến (mutations) GraphQL được sinh tự động
