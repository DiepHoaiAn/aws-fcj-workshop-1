---
title : "Tích hợp đăng nhập thật với Cognito"
date : "2025-06-14" 
weight : 4 
chapter : false
pre : " <b> 2.4 </b> "
---
Sau khi đã khởi tạo Cognito trong Amplify và đồng bộ về local, bước này sẽ giúp bạn **thay thế hoàn toàn đăng nhập giả lập** bằng đăng nhập thực tế với Amazon Cognito.

---

### Mục tiêu

- Cài thư viện AWS Amplify cho Vue 3
- Sử dụng Authenticator UI từ `@aws-amplify/ui-vue` để xử lý đăng nhập
- Cập nhật `Pinia Store` để lưu trạng thái người dùng đã đăng nhập
- Xóa logic mock login và thay bằng real login

---
### Các bước thực hiện

#### 1. Cài đặt thư viện

Chạy lệnh sau trong thư mục gốc (nơi có `package.json`):

```bash
npm install aws-amplify @aws-amplify/ui-vue
```
#### 2. Đổi tên file cấu hình Amplify
Amplify tạo ra file aws-exports.js chứa cấu hình kết nối với dịch vụ AWS. Để dễ quản lý, bạn nên đổi tên file này thành `amplify-config.js` hoặc `aws-exports.ts` nếu bạn sử dụng TypeScript.  
Mở terminal và chạy lệnh sau:

```bash
mv src/aws-exports.js src/amplify-config.js
```
Hoặc bạn muốn đổi tên thủ công file `aws-exports.js` thành `amplify-config.js` hoặc `aws-exports.ts` nếu bạn sử dụng TypeScript.  
Trong VS Code, bạn có thể chuột phải vào file `aws-exports.js` và chọn **Rename** để đổi tên.

#### 3. Cập nhật **AppBar.vue**
File: src/components/AppBar.vue
Thêm vào cuối script setup:
```ts
const auth = useAuthStore();
const userSignedIn = computed(() => auth.userAuthenticated);

onMounted(() => {
  auth.checkPreviousUserSignedIn();
});
```
#### 4. Cập nhật **auth.ts** (Pinia Store)
File: src/store/auth.ts
```ts
import { defineStore } from 'pinia';
import { getCurrentUser } from 'aws-amplify/auth';
import { Hub } from 'aws-amplify/utils';

export const useAuthStore = defineStore('auth', {
  state: () => ({
    userAuthenticated: false,
    userId: '',
  }),
  actions: {
    async checkPreviousUserSignedIn() {
      try {
        const user = await getCurrentUser();
        this.userAuthenticated = true;
        this.userId = user.userId;
      } catch {
        this.userAuthenticated = false;
        this.userId = '';
      }
    }
  }
});

// Theo dõi các sự kiện đăng nhập/đăng xuất
Hub.listen('auth', async (data) => {
  const store = useAuthStore();
  switch (data.payload.event) {
    case 'signedIn':
      const user = await getCurrentUser();
      store.userAuthenticated = true;
      store.userId = user.userId;
      break;
    case 'signedOut':
      store.userAuthenticated = false;
      store.userId = '';
      break;
  }
});
```
#### 5. Cập nhật **SignInOutDialogs.vue**
File: File: src/components/SignInOutDialogs.vue
```ts
<script setup lang="ts">
import { DialogState } from '@/types/DialogState';
import { inject } from 'vue';
import { useAuthStore } from '@/store/auth';
import { signOut } from 'aws-amplify/auth';

import { Authenticator } from "@aws-amplify/ui-vue";
import "@aws-amplify/ui-vue/styles.css";

import { Amplify } from 'aws-amplify';
import awsconfig from '../aws-exports';
Amplify.configure(awsconfig);

const authDialogSignOutState = inject<DialogState>('authDialogSignOutState', { showDialog: false });
const authDialogSignInState = inject<DialogState>('authDialogSignInState', { showDialog: false });

const authStore = useAuthStore();

function closeDialog() {
  authDialogSignOutState.showDialog = false;
  authDialogSignInState.showDialog = false;
}

function signIn() {
  authStore.userAuthenticated = true;
  authDialogSignInState.showDialog = false;
}

function signOutUser() {
  signOut();
  authStore.userAuthenticated = false;
  authDialogSignOutState.showDialog = false;
}
</script>
```
Template phần đăng nhập:
```vue
<template>
  <v-dialog v-model="authDialogSignInState.showDialog" width="unset" transition="dialog-top-transition">
    <authenticator>
      <template v-slot="{ user }">
        {{ signIn() }}
      </template>
    </authenticator>
  </v-dialog>

  <v-dialog v-model="authDialogSignOutState.showDialog" width="unset" transition="dialog-top-transition">
    <v-card title="Sign Out?" color="warning">
      <v-card-text>
        Are you sure you want to sign out?
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn text="Sign Out" @click="signOutUser"></v-btn>
        <v-btn text="Cancel" @click="closeDialog"></v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
```

---
### Kiểm thử
1. Chạy lại ứng dụng:
   ```bash
   npm run dev
   ```
2. Mở trình duyệt và truy cập `http://localhost:3000`.
3. Nhấn nút **Sign In** để mở giao diện đăng nhập của Amplify.
4. Nhấn Create Account, nhập email và mật khẩu thật (email có thể nhận mã xác thực)  
![Create Account](/images/2.prerequisite/04-SignIn1.png)
![Create Account](/images/2.prerequisite/04-SignIn2.png)
![Create Account](/images/2.prerequisite/04-SignIn3.png)
5. Kiểm tra email và xác thực  
![Create Account](/images/2.prerequisite/04-SignIn4.png)
![Create Account](/images/2.prerequisite/04-SignIn5.png)
6. Bạn đã đăng nhập thành công! Bây giờ có thể đặt vé sự kiện như người dùng thật. 
![Create Account](/images/2.prerequisite/04-SignIn6.png)

{{% notice tip %}}
Bạn có thể tuỳ chỉnh giao diện đăng nhập của Authenticator sau khi đã hoạt động ổn định.
Amplify UI hỗ trợ custom rất linh hoạt.
{{% /notice %}}

### Kết luận
Bạn đã thay thế thành công mock login bằng đăng nhập thực sử dụng **Amazon Cognito**.  
**Amplify UI Authenticator** giúp triển khai nhanh, an toàn, dễ bảo trì.  
**Pinia Store** đã được cập nhật để theo dõi trạng thái đăng nhập thực tế.  

Tiếp theo: Di chuyển toàn bộ dữ liệu mô phỏng (mock data) đang lưu local lên cloud, sử dụng một dịch vụ backend thực tế.


