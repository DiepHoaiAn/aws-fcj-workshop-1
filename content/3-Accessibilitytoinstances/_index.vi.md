---
title : "Truy xuất dữ liệu với Amazon DynamoDB"
date : "2025-06-14"
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---
Sau khi triển khai thành công xác thực người dùng với Amazon Cognito, nhóm kỹ thuật của bạn tại **FCJ-UTE Computing** yêu cầu bạn thực hiện bước tiếp theo:  

> Di chuyển toàn bộ dữ liệu mô phỏng (mock data) đang lưu local lên cloud, sử dụng một dịch vụ backend thực tế.


Yêu cầu cụ thể là sử dụng **AWS Amplify** kết hợp **Amazon DynamoDB** để lưu trữ dữ liệu.

---

### Mục tiêu chức năng

Người dùng của ứng dụng MUSSEL sẽ có thể:

- Xem danh sách sự kiện sắp tới hoặc đã diễn ra.
- Xem địa điểm tổ chức của sự kiện.
- Biết số lượng vé còn lại.
- Kiểm tra các sự kiện mình đã đăng ký.

Hiện tại, toàn bộ dữ liệu này vẫn là **giả lập (mock)** và được lưu tạm thời trong local memory. Ta cần chuyển nó lên cloud.

---

### Cơ sở dữ liệu và NoSQL

Để lưu trữ dữ liệu dạng động, ta cần một dịch vụ cơ sở dữ liệu. Trong kiến trúc serverless, lựa chọn phổ biến là:

- **Amazon DynamoDB** – một cơ sở dữ liệu NoSQL được quản lý hoàn toàn.
- Thao tác thông qua các hàm **CRUD** (Create, Read, Update, Delete).
- Tích hợp dễ dàng với **AWS Amplify** và GraphQL API.

---

### Mô hình dữ liệu (Entities & Attributes)

Chúng ta sẽ cần các thực thể sau:

#### Event

- *id*: ID của sự kiện (bắt buộc)
- *name*: Tên sự kiện
- *description*: Mô tả
- *event_owner*: Người tổ chức
- *room_id*: Mã phòng tổ chức
- *image_file_name*: Tên file ảnh minh họa
- *event_datetime_start*, *event_datetime_end*
- *event_duration*, *total_tickets*
- *tickets*: Mảng *user_id* đã đặt vé

#### Room

- *id*: ID phòng
- *name*: Tên phòng
- *capacity*: Sức chứa
- *bookings*: Danh sách các lần đã đặt phòng (*datetime_start*, *datetime_end*)

#### User (tạm thời implicit thông qua Cognito)

- *user_id*: ID người dùng (chính là Cognito sub)
- Dữ liệu user sẽ không lưu trực tiếp mà tham chiếu trong *tickets[]* của Event.

{{% notice tip %}}
Ta sẽ chưa tạo bảng riêng cho User, vì thông tin người dùng được xử lý bởi Cognito.
{{% /notice %}}

---

### Thay đổi luồng truy xuất dữ liệu

Hiện tại, app đang dùng logic sau:

```ts
dataStore.initMockData(); // sinh dữ liệu giả
```
Sắp tới, chúng ta sẽ thay thế bằng truy vấn GraphQL thực sự để lấy dữ liệu từ DynamoDB:
```ts
const rooms = await apiClient.graphql({ query: listRooms });
const events = await apiClient.graphql({ query: listEvents });

this.rooms = rooms.data.listRooms.items;
this.events = events.data.listEvents.items;
```
### Sơ đồ kiến trúc kết nối với DynamoDB

![arch-diagram](/images/3.connect/System_Architecture.png)

{{% notice tip %}}
- App gửi truy vấn/mutation tới AppSync
- AppSync xử lý và đọc/ghi dữ liệu từ DynamoDB
- Cognito kiểm tra token xác thực khi cần
{{% /notice %}}
{{% notice info %}}
Đây là luồng dữ liệu chính giúp frontend hoạt động với cơ sở dữ liệu thực, thay vì mock data trước đó.
{{% /notice %}}

###  Cập nhật dữ liệu (ví dụ: đặt vé)
Trước đây, khi đặt vé: `event.bookTicket(userId);`  
Giờ ta cần đồng bộ thay đổi đó lên cloud:
```ts
const input = {
  id: event.id,
  tickets: [...event.tickets, userId]
};

await apiClient.graphql({
  query: updateEvent,
  variables: { input }
});
```

{{% notice warning %}}
Chỉ user đã đăng nhập mới được phép thực hiện mutation này.
{{% /notice %}}

### Kết luận
Bạn đã hiểu rõ lý do và cách đưa dữ liệu từ frontend lên một hệ thống backend thật sự.  
DynamoDB sẽ thay thế mock data và trở thành nguồn dữ liệu chính.  
Việc truy xuất (query) và cập nhật (mutation) dữ liệu sẽ thực hiện thông qua GraphQL API.  
Mọi thao tác sẽ gắn chặt với cơ chế phân quyền xác thực dựa trên Amazon Cognito.  

Tiếp theo: Bạn sẽ thực hiện định nghĩa các thực thể dữ liệu này trong schema GraphQL, deploy lên AWS và tích hợp vào frontend.
### Nội dung tiếp theo
- [3.1 Mô hình hóa dữ liệu với AWS Amplify và GraphQL](./3.1-ModellingEntities/)
- [3.2 Thêm các truy vấn GraphQL để kết nối dữ liệu thực](./3.2-AddQueries/)