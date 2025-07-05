---
title : "Khám phá ứng dụng MUSSEL"
date : "2025-06-14"
weight : 1 
chapter : false
pre : " <b> 2.1 </b> "
---

Ở bước này, bạn sẽ chạy thử ứng dụng mẫu MUSSEL trên localhost và tìm hiểu giao diện, hành vi và dữ liệu mô phỏng. Đây là cơ sở để bạn hiểu cách frontend hoạt động trước khi tích hợp với backend thật.

---

### Khởi chạy ứng dụng

Sau khi hoàn tất các bước clone và cài đặt (ở phần trước), chạy lệnh sau để khởi chạy frontend:

```bash
npm run dev
```
Sau vài giây, bạn sẽ thấy kết quả:
```
  Vite v4.6.0  ready in 300 ms

  ➜ Local:   http://localhost:3000/
  ➜ Network: use `--host` to expose
```
→ Mở trình duyệt và truy cập địa chỉ http://localhost:3000/ để bắt đầu khám phá.

### Giao diện chính

Khi truy cập thành công, bạn sẽ thấy ứng dụng hiển thị danh sách các sự kiện. Giao diện gồm các phần:

![Giao diện ứng dụng MUSSEL](/images/2.prerequisite/front-end1.png)

- **Thanh điều hướng trên cùng (App Bar)** với các nút: chuyển theme, giới thiệu, đăng nhập.
- **Danh sách sự kiện**: hiển thị các sự kiện mẫu (mock).
- **Thanh điều hướng dưới cùng** với 3 mục: Past Events, My Tickets, Upcoming Events.

---

### Trải nghiệm luồng người dùng

#### Khi chưa đăng nhập

- Nút **Sign In** hiển thị ở góc trên phải.
- Nút **My Tickets** bị vô hiệu hóa.
- Không thể đặt vé cho bất kỳ sự kiện nào.

Nếu bạn thử nhấn vào nút `Book Ticket`, hệ thống sẽ hiện thông báo: **Sign in to book tickets**

{{% notice info %}}
Dữ liệu trên giao diện là *mock data* – được tạo mỗi lần chạy `npm run dev`, bao gồm các sự kiện từ **2 ngày trước đến 2 ngày sau** hiện tại.
{{% /notice %}}


#### Đăng nhập (giả lập)

Hiện tại, chức năng đăng nhập vẫn là **giả lập (mock login)**. Để thử:

1. Nhấn nút **Sign In**.
2. Nhập tên bất kỳ vào hộp thoại (ví dụ: `alice`).
3. Sau khi đăng nhập:
   - Nút **My Tickets** được kích hoạt.
   - Bạn có thể đặt vé cho các sự kiện sắp tới.
   - Khi nhấn **My Tickets**, bạn sẽ thấy danh sách vé đã đặt.

{{% notice tip %}}
Việc đăng nhập giả lập giúp kiểm thử luồng người dùng trước khi tích hợp xác thực thật bằng Amazon Cognito.
{{% /notice %}}


### Một số thao tác bạn nên thử

| Tình huống               | Kết quả mong đợi                                                                 |
|--------------------------|----------------------------------------------------------------------------------|
| Truy cập **Past Events** | Hiển thị sự kiện đã kết thúc, **không thể đặt vé**                              |
| Truy cập **Upcoming Events** | Có thể đặt vé **nếu đã đăng nhập**, nếu không sẽ hiện thông báo yêu cầu đăng nhập |
| Chuyển theme **Dark/Light** | Giao diện chuyển đổi giữa chế độ sáng và tối ngay lập tức                     |
| Nhấn **Sign Out**        | Quay lại trạng thái chưa đăng nhập, `My Tickets` bị vô hiệu hóa                 |


---
### Dữ liệu do AI tạo ra

Hình ảnh và mô tả sự kiện trong mock data được tạo bằng **Generative AI**, sử dụng **Amazon Bedrock** và **Amazon Titan**.

---
### Kết luận

Sau bước này, bạn đã:

- Chạy thử ứng dụng **MUSSEL** trên local.
- Hiểu được cách giao diện hoạt động với dữ liệu mô phỏng.
- Trải nghiệm luồng người dùng: **đăng nhập**, **đặt vé**, **xem vé đã đặt**.

Ở bước tiếp theo, bạn sẽ kết nối ứng dụng với backend thực bằng cách thêm xác thực người dùng bằng **Amazon Cognito**.

