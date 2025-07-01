# 1. GIỚI THIỆU 

## 1.1. Bối cảnh

Trong quá trình học và tìm hiểu về kiến trúc *serverless*, em mong muốn triển khai một hệ thống xử lý ảnh tự động trên nền tảng **AWS**. Hệ thống này được xây dựng với mục tiêu mô phỏng một **quy trình xử lý ảnh đại diện người dùng** – điều mà rất nhiều ứng dụng thực tế như mạng xã hội, học trực tuyến, hoặc nền tảng đăng ký sự kiện thường cần đến.

![Bussiness Context](/images/1.introduction/Bussiness_Context.jpg)

### **Tình huống ứng dụng**

Trường hợp áp dụng: hệ thống này có thể sử dụng trong việc quản lý hồ sơ sinh viên, nơi mỗi sinh viên chỉ được upload ảnh đại diện hợp lệ, không trùng nhau, và được lưu lại thời gian xác thực. Đây là tình huống gần gũi với các hệ thống tuyển sinh, quản lý tài khoản học viên, hoặc các ứng dụng eKYC đơn giản.

Giả sử chúng ta đang xây dựng một nền tảng quản lý hồ sơ học viện, mỗi học viên cần tải lên ảnh đại diện khi tạo tài khoản. Để đảm bảo ảnh hợp lệ, không bị trùng lặp, và hệ thống không bị đầy bởi các ảnh không cần thiết, ta cần thiết kế luồng xử lý ảnh như sau:

1. Người dùng tải ảnh đại diện lên thông qua một giao diện web đơn giản.  
2. Hệ thống sẽ kiểm tra định dạng ảnh, phát hiện khuôn mặt, đảm bảo không bị trùng với người khác đã đăng ký trước đó.  
3. Nếu hợp lệ, ảnh sẽ được thu nhỏ thành thumbnail, lưu metadata, và gửi email xác nhận thành công.  
4. Nếu không hợp lệ (ví dụ như không thấy mặt người, hoặc trùng ảnh), hệ thống sẽ gửi email thông báo lỗi và không tiếp tục xử lý.

---

### **Luồng xử lý tự động**

Toàn bộ quy trình xử lý được thực hiện hoàn toàn tự động, theo luồng sau:

#### **1. Upload ảnh**
- Người dùng tải ảnh qua trang web tĩnh đơn giản (S3 Static Website).
- Ảnh được lưu vào một bucket S3.

![Upload Web](images/image_upload.png)

#### **2. Trigger workflow**
- Sự kiện upload ảnh kích hoạt một luồng xử lý thông qua **AWS Step Functions** và **EventBridge**.

#### **3. Các bước xử lý tuần tự**
- **Kiểm tra định dạng ảnh**: chỉ chấp nhận `.jpg` hoặc `.png`.  
- **Nhận diện khuôn mặt**: sử dụng **Amazon Rekognition**.  
- **Kiểm tra trùng lặp**: đối chiếu khuôn mặt với các ảnh đã có.  
- **Xử lý song song**:
    - Tạo ảnh thumbnail (Lambda resize ảnh).  
    - Ghi khuôn mặt vào collection Rekognition.
- **Ghi metadata**: lưu thông tin ảnh và thời gian xử lý (`processed_at`) vào **DynamoDB**.  
- **Xoá ảnh gốc** để tiết kiệm dung lượng.  
- **Gửi email xác nhận thành công** (SNS).

#### **4. Xử lý lỗi**
- Nếu bất kỳ bước nào thất bại, hệ thống sẽ gửi email báo lỗi thông qua một topic SNS khác.

![Step Function Flow](images/state_machine_flow_3.png)

> *Sơ đồ luồng điều phối bên trong Step Functions, minh hoạ rõ các nhánh xử lý ảnh – từ kiểm tra định dạng, nhận diện khuôn mặt, kiểm tra trùng lặp, xử lý song song (resize + index) đến lưu metadata và gửi thông báo.*

---

### **Mục tiêu và giá trị học được**

Thông qua quá trình tự triển khai này, chúng ta sẽ học được:

- Cách thiết kế một luồng xử lý bất đồng bộ, theo sự kiện (event-driven)  
- Sử dụng **Step Functions** để điều phối nhiều dịch vụ AWS  
- Tổ chức hệ thống gọn gàng, tiết kiệm tài nguyên và tối ưu chi phí  
- Đảm bảo phản hồi với người dùng qua email xác nhận hoặc thông báo lỗi
