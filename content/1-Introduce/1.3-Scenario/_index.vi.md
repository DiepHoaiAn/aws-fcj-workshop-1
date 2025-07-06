---
title : "Kịch bản: Ứng dụng sự kiện MUSSEL"
date : "2025-06-14"
weight : 3 
chapter : false
pre : " <b> 1.3 </b> "
---
### Giới thiệu bối cảnh

Chúc mừng, bạn vừa được tuyển dụng vào làm tại **FCJ-UTE Computing** với vị trí kỹ sư phần mềm!

Trong dự án này, bạn sẽ làm việc với một ứng dụng web có tên là **MUSSEL** (My University’s Sports & Social Events List) – nền tảng giúp sinh viên dễ dàng xem và đặt vé tham gia các sự kiện trong trường đại học.


### Mô tả ứng dụng

- MUSSEL là một **ứng dụng web mẫu** được xây dựng bởi **AWS Workshop Studio team**, nhằm mục tiêu hỗ trợ huấn luyện và đào tạo các kỹ năng triển khai cloud hiện đại với **AWS Amplify**.
- Ứng dụng được thiết kế sẵn như một **prototype chưa hoàn chỉnh**, chỉ chạy được ở chế độ local với dữ liệu giả lập.
- Bạn sẽ đóng vai trò như một kỹ sư phần mềm mới, tiếp nhận ứng dụng mẫu này và **tự tay tích hợp các dịch vụ cloud để hoàn thiện chức năng backend**.


### Mục tiêu của bài lab

{{% notice info %}}
Workshop này **không nhằm mục đích hướng dẫn lập trình giao diện frontend từ đầu**.  
Toàn bộ mã nguồn giao diện đã được cung cấp sẵn bởi nhóm phát triển của AWS Studio.
{{% /notice %}}

Thay vào đó, trọng tâm của lab là thực hành tích hợp và kết nối các dịch vụ AWS với frontend mẫu thông qua các bước như:

- Khởi tạo môi trường cloud sử dụng **AWS Amplify**.
- Thêm **xác thực người dùng** bằng **Amazon Cognito**.
- Tạo **cơ sở dữ liệu động** bằng **Amazon DynamoDB + GraphQL API**.
- Kết nối frontend có sẵn với các backend service thật qua các mutation/query thực tế.

{{% notice tip %}}
Thông qua quá trình này, sinh viên sẽ tiếp cận toàn bộ quy trình triển khai một ứng dụng serverless thực tế.  
Là bước nền tảng cho kỹ năng quan trọng với các doanh nghiệp cloud-native hiện nay.
{{% /notice %}}

---

### Giao diện ứng dụng MUSSEL (ban đầu - dữ liệu giả)

![Giao diện ứng dụng MUSSEL](/images/1.introduction/scenario-app-preview.png)

Giao diện hiển thị danh sách các sự kiện mẫu với hình ảnh, thông tin chi tiết, trạng thái đặt vé,...  
Các dữ liệu này được tạo ngẫu nhiên từ script mô phỏng (mock data) khi chạy app ở chế độ `npm run dev`.


#### Tình trạng ban đầu

- Ứng dụng chỉ chạy được local.
- Không có tính năng đăng nhập thật.
- Dữ liệu không được lưu – mọi hành động mất khi refresh.


#### Nhiệm vụ được giao

Bạn sẽ là người thực hiện các bước sau:

1. Khởi tạo Amplify project cho frontend đã có.
2. Thêm dịch vụ xác thực người dùng (Auth) với Amazon Cognito.
3. Thiết kế mô hình dữ liệu (schema GraphQL) cho các thực thể như *Event*, *Room*, *Booking*.
4. Tích hợp và đồng bộ cơ sở dữ liệu thật sử dụng DynamoDB.
5. Gắn kết toàn bộ các thao tác frontend (sign in, đặt vé, tạo dữ liệu mẫu) với backend thực tế qua API.

#### Sơ đồ quy trình thực hành

Đây là sơ đồ minh họa các bước bạn sẽ thực hiện trong workshop này:

![Workflow - Amplify Integration](/images/1.introduction/workflow-overview.png)

> Sơ đồ thể hiện toàn bộ hành trình từ việc thiết lập môi trường local, cấu hình backend bằng AWS Amplify, đến tích hợp frontend với backend thật.  
Kết quả cuối cùng là một ứng dụng MUSSEL full-stack chạy ổn định với dữ liệu thực trên cloud.

Các bước chính trong quy trình:
1. Clone ứng dụng mẫu MUSSEL từ GitHub và khởi chạy thử local.
2. Khởi tạo môi trường AWS Amplify  cho project frontend.
3. Thêm dịch vụ xác thực người dùng bằng Amazon Cognito.
4. Thiết kế schema dữ liệu GraphQL (Event, Room, Booking).
5. Tự động tạo backend thật với DynamoDB và GraphQL API (amplify push).
6. Kết nối frontend có sẵn với API backend bằng mutation/query.
7. Kiểm thử hoàn chỉnh luồng người dùng: đăng nhập, đặt vé, lưu dữ liệu.
8. Ứng dụng full-stack serverless hoàn tất*.


{{% notice info %}}
Ứng dụng này là sản phẩm mẫu do đội ngũ AWS Studio phát triển, được cung cấp sẵn phần giao diện để phục vụ mục tiêu đào tạo.
Tuy nhiên, toàn bộ quy trình kết nối cloud, cấu hình dịch vụ, triển khai và kiểm thử người học phải tự tay thực hiện trong suốt quá trình workshop.
{{% /notice %}}