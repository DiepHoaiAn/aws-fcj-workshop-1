---
title : "Khám phá mã nguồn ứng dụng MUSSEL"
date : "2025-06-14" 
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---
Mục tiêu phần này là giúp bạn hiểu cấu trúc mã nguồn và cơ chế xử lý đăng nhập (mock login) của ứng dụng MUSSEL trước khi tích hợp Cognito.

---

### Cấu trúc thư mục chính

| Thành phần              | Chức năng chính                                     |
|-------------------------|-----------------------------------------------------|
| public/               | Lưu ảnh minh họa sự kiện                            |
| index.html            | Điểm khởi đầu của ứng dụng                          |
| src/main.ts           | Khởi tạo app Vue và load mock data                 |
| src/App.vue           | Thành phần gốc của giao diện                        |
| src/components/       | Giao diện: AppBar, BottomNav, EventsList,...        |
| src/store/            | Quản lý trạng thái (auth, data, UI, mock data)     |
| src/models/           | Kiểu dữ liệu (Event, Room)                          |
| src/plugins/          | Cấu hình Vuetify, WebFont,...                       |


### Luồng khởi tạo chính

- **index.html** nhúng **main.ts**
- **main.ts** gọi **dataStore.initMockData()** để tạo dữ liệu giả
- Tải **App.vue**, hiển thị cấu trúc ứng dụng


### Cách xử lý nút Sign In

#### File AppBar.vue

```vue
<v-btn v-if="userSignedIn" @click="showSignOut">Sign Out</v-btn>
<v-btn v-else @click="showSignIn">Sign In</v-btn>
<SignInOutDialogs />
```
Dựa vào biến `userSignedIn` từ `authStore.userAuthenticated`

Khi nhấn **Sign In** → gọi **showSignIn()** → hiển thị hộp thoại

#### File SignInOutDialogs.vue  

Hiển thị `v-dialog` khi `showDialog = true`. Không gọi API, chỉ gán:

```ts
function signIn() {
  authStore.userAuthenticated = true;
}
```
→ Mô phỏng trạng thái đã đăng nhập.

{{% notice info %}}
Chức năng Sign In ở bước này chỉ là giả lập (mock).
Bạn sẽ thay thế bằng hệ thống xác thực thực tế với Amazon Cognito trong các bước tiếp theo.
{{% /notice %}}

### Kết luận
- Mã nguồn được tổ chức rõ ràng theo cấu trúc component-based.  
- Sử dụng Vue 3 + Pinia để quản lý trạng thái.  
- Cơ chế đăng nhập hiện tại chưa kết nối với backend – chỉ mô phỏng về mặt giao diện.  

> Tiếp theo: Bắt đầu thêm xác thực thực bằng cách tích hợp Amazon Cognito với Amplify.