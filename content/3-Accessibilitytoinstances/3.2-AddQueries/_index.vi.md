---
title : "Thêm các truy vấn GraphQL để kết nối dữ liệu thực"
date : "2025-06-14"
weight : 2 
chapter : false
pre : " <b> 3.2. </b> "
---
Sau khi đã định nghĩa schema dữ liệu và đẩy lên DynamoDB thông qua Amplify CLI, bước tiếp theo là **kết nối frontend với cơ sở dữ liệu thật** bằng cách sử dụng các **truy vấn và đột biến (mutations) GraphQL** được sinh tự động.

---

### Mục tiêu

- Kết nối frontend với DynamoDB bằng API GraphQL do Amplify cung cấp.
- Thay thế dữ liệu mô phỏng bằng dữ liệu thật từ backend.
- Cập nhật hàm *bookTicket()* để lưu lại vé đã đặt vào DynamoDB.

--- 
### Cấu trúc GraphQL được Amplify sinh tự động

Sau khi chạy `amplify push`, thư mục *src/graphql/* sẽ được tạo. Bên trong có:

- *mutations.ts*: Định nghĩa các thao tác cập nhật dữ liệu (create, update, delete)
- *queries.ts*: Định nghĩa các truy vấn (get, list)
- *subscriptions.ts*: Cho phép real-time updates (không dùng trong lab này)
- *schema.json*: Toàn bộ schema ở dạng JSON

---

### Cập nhật mã nguồn *data.ts*

#### Bước 1: Thêm các import cần thiết (trên đầu file)

```ts
// Amplify GraphQL
import { listEvents, listRooms } from '@/graphql/queries';
import { deleteRoom, deleteEvent, createEvent, createRoom, updateEvent } from '@/graphql/mutations';
import { generateClient } from 'aws-amplify/api';

// Tạo client gọi API GraphQL
const apiClient = generateClient();
```

#### Bước 2: Thay thế hàm fetchData()
```ts
async fetchData() {
  const dbRooms = await apiClient.graphql({ query: listRooms });
  const dbEvents = await apiClient.graphql({ query: listEvents });

  this.rooms = dbRooms.data.listRooms.items.map(room => {
    return new Room(room.id, room.name, room.capacity, []);
    // 📌 Gợi ý: bạn có thể viết truy vấn tùy chỉnh để lấy thêm bookings
  });

  this.events = dbEvents.data.listEvents.items.map(event => {
    let ev = new EventDetails(
      event.id,
      event.name,
      event.description,
      event.event_owner,
      event.room_id,
      event.image_file_name,
      new Date(event.event_datetime_start),
      new Date(event.event_datetime_end),
      event.event_duration,
      event.total_tickets,
      event.tickets
    );
    event.tickets.forEach(ticket => {
      ev.bookTicket(ticket);
    });
    return ev;
  });
}
```
#### Bước 3: Cập nhật hàm bookTicket()
```ts
async bookTicket(eventId: string, studentId: string) {
  const event = this.events.find(e => e.id === eventId);
  if (event && event.total_tickets > event.tickets.length) {
    event.bookTicket(studentId);

    // Đồng bộ vé đã đặt lên DynamoDB
    const input = {
      id: event.id,
      tickets: event.tickets,
    };

    const updatedEvent = await apiClient.graphql({
      query: updateEvent,
      variables: { input }
    });

    console.log('Ticket booked:', updatedEvent);
  }
}
```

---

### Chạy thử ứng dụng
Để kiểm tra kết nối với backend, bạn cần chạy ứng dụng frontend. Mở terminal và thực hiện các bước sau:  
Bước 1. Đảm bảo bạn đang ở thư mục gốc của dự án (nơi có *package.json*).  
Bước 2. Chạy lệnh sau để cài đặt các phụ thuộc:
```bash
npm run dev
```
Bước 3. Sau khi quá trình cài đặt hoàn tất, bạn sẽ thấy thông báo:
```Vite v4.6.0  ready in 300 ms
➜ Local:   http://localhost:3000/
➜ Network: use `--host` to expose
```
Bước 4. Mở trình duyệt và truy cập địa chỉ [http://localhost:3000](http://localhost:3000) và thực hiện các bước sau:

- Đăng nhập.
![Login](/images/3.connect/02-LogIn.png) 
> Khi đăng nhập, bạn sẽ thấy nút **My Tickets** được kích hoạt.  

![Login](/images/3.connect/02-LogIn1.png) 
- Vào nút About App (góc trên bên phải), bạn sẽ thấy hộp thoại hiển thị các tùy chọn:
  - Generate Mock Data: Tạo dữ liệu mô phỏng để kiểm thử.
  - Upload Mock Data to DynamoDB: Đẩy dữ liệu mô phỏng lên DynamoDB.
  - Chọn Close để đóng hộp thoại.  
![About](/images/3.connect/02-LogIn2.png) 
- Chọn:  
  - Generate Mock Data: Tạo dữ liệu mô phỏng để kiểm thử. Khi này, bạn sẽ thấy danh sách sự kiện và phòng hiển thị từ dữ liệu mô phỏng.
![Data](/images/3.connect/02-LogIn3.png) 
  - Chọn Close để đóng hộp thoại.
Sau đó bạn sẽ thấy danh sách sự kiện và phòng hiển thị từ dữ liệu thực trong DynamoDB.  
![Login](/images/3.connect/02-LogIn4.png) 

- Chọn một sự kiện và đặt vé bằng nút Book Ticket.
![Book Ticket](/images/3.connect/02-BookTicket.png) 
- Sau khi đặt vé, bạn có thể thấy mục Ticket Availables giảm xuống và vé đã đặt sẽ được lưu trong DynamoDB.
![Book Ticket](/images/3.connect/02-BookTicket1.png) 
- Vào mục My Tickets, bạn sẽ thấy vé vừa đặt được hiển thị.
![Book Ticket](/images/3.connect/02-BookTicket2.png)

{{% notice tip %}}
Sau khi ứng dụng đã kết nối thành công với backend, bạn có thể truy cập AWS Console để **xác nhận dữ liệu đã được ghi vào DynamoDB**.
{{% /notice %}}

---

### Xem dữ liệu trong DynamoDB
Bước 1: Truy cập AWS Console  
- Truy cập: [https://console.aws.amazon.com/](https://console.aws.amazon.com/)  
- Đăng nhập bằng tài khoản AWS bạn đã dùng khi khởi tạo Amplify.  

Bước 2: Chọn đúng khu vực (Region)  
- Ở góc trên bên phải, chọn **Asia Pacific (Singapore) - `ap-southeast-1`** để có tốc độ truy cập nhanh nhất.
{{% notice warning %}}  
Đây là region bạn đã chọn khi init Amplify, nên cần đảm bảo đúng để thấy được tài nguyên. 
{{% /notice %}}

Bước 3: Mở dịch vụ DynamoDB  
- Gõ `DynamoDB` vào thanh tìm kiếm trên đầu trang.  
- Chọn kết quả **DynamoDB**.  
![DB](/images/3.connect/03-DB.png)
Bước 4: Xem các bảng dữ liệu  
- Ở sidebar bên trái → chọn **Tables**.  
- Tìm các bảng có tên như:  
```text
Event-XXXXX-dev
Room-XXXXX-dev
```
{{% notice warning %}}
Tên bảng có thể khác nhau tùy theo tên dự án và môi trường (dev, staging...).
{{% /notice %}}

- Chọn bảng **eventplanner-dev-Event-XXXXX** để xem dữ liệu sự kiện.
- Bạn sẽ thấy các mục dữ liệu đã được tạo từ ứng dụng, bao gồm các trường như *id*, *name*, *description*, *event_owner*, *room_id*, *event_datetime_start*, *event_datetime_end*, *total_tickets*, và mảng *tickets* chứa các ID người dùng đã đặt vé.
- Tương tự, bạn có thể chọn bảng *Room-XXXXX-dev* để xem dữ liệu phòng.
![DB](/images/3.connect/03-DB1.png)


{{% notice info %}}
Bạn đã hoàn thành việc thay thế toàn bộ dữ liệu mock bằng hệ thống dữ liệu thật được lưu trữ trên đám mây với DynamoDB.
{{% /notice %}}

{{% notice tip %}}
Bạn có thể tiếp tục mở rộng ứng dụng với các tính năng như: Tạo/Xoá sự kiện từ UI, Kết nối hình ảnh sự kiện với Amazon S3, Thêm phân quyền truy cập theo vai trò người dùng.  
Những phần này có thể sẽ được thực hiện trong các workshop sau.
{{% /notice %}}

---

### Kết luận

- Ứng dụng frontend đã được kết nối thành công với DynamoDB thông qua các truy vấn và mutation GraphQL do Amplify sinh tự động.  
- Các chức năng như xem danh sách sự kiện, xem phòng, và đặt vé hiện đang sử dụng dữ liệu thực trên cloud thay vì dữ liệu mô phỏng (mock).  
- Mọi hành động của người dùng, như đặt vé, đều được ghi nhận và đồng bộ hai chiều giữa client và hệ thống backend trên AWS.  
