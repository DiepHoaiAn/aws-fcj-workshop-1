---
title : "Integrating Real Login with Cognito"
date : "2025-06-14" 
weight : 4 
chapter : false
pre : " <b> 2.4 </b> "
---

After initializing Cognito with Amplify and syncing the configuration locally, this step will guide you to **completely replace the mock login system** with real authentication using Amazon Cognito.

---

### Objective

- Install the AWS Amplify library for Vue 3  
- Use the Authenticator UI from `@aws-amplify/ui-vue` to handle login  
- Update the `Pinia Store` to manage logged-in user state  
- Remove mock login logic and replace it with real login

---

### Implementation Steps

#### 1. Install required libraries

Run the following command from the root directory (where `package.json` is located):

```bash
npm install aws-amplify @aws-amplify/ui-vue
```
#### 2. Rename Amplify config file
Amplify generates a file named aws-exports.js that contains your AWS service configuration. To keep things organized, you should rename this file to amplify-config.js, or aws-exports.ts if you're using TypeScript.
Open your terminal and run:
```bash
mv src/aws-exports.js src/amplify-config.js
```
Alternatively, you can manually rename `aws-exports.js` to amplify-config.js` or `aws-exports.ts` if you are using TypeScript.
In VS Code, right-click the `aws-exports.js` file and choose **Rename** to change its name.

#### 3. Update **AppBar.vue**
File: src/components/AppBar.vue
Add at the end of script setup:  
```ts
const auth = useAuthStore();
const userSignedIn = computed(() => auth.userAuthenticated);

onMounted(() => {
  auth.checkPreviousUserSignedIn();
});
```
#### 4. Update **auth.ts** (Pinia Store)
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

// Monitor login/logout events
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
#### 5. Update **SignInOutDialogs.vue**
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
Login template:
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
### Testing

1. Restart the app:
   ```bash
   npm run dev
   ```

2. Open your browser and navigate to `http://localhost:3000`.  
3. Click the Sign In button to open the Amplify login interface.  
4. Click Create Account, enter a valid email and password (you will receive a verification code via email)  
![Create Account](/images/2.prerequisite/04-SignIn1.png)
![Create Account](/images/2.prerequisite/04-SignIn2.png)
![Create Account](/images/2.prerequisite/04-SignIn3.png)
5. Check email and verify  
![Create Account](/images/2.prerequisite/04-SignIn4.png)
![Create Account](/images/2.prerequisite/04-SignIn5.png)  
6. You have successfully logged in! You can now book event tickets as a real user.  
![Create Account](/images/2.prerequisite/04-SignIn6.png)  

{{% notice tip %}}
Bạn có thể tuỳ chỉnh giao diện đăng nhập của Authenticator sau khi đã hoạt động ổn định.
Amplify UI hỗ trợ custom rất linh hoạt.
{{% /notice %}}

### Conclusion

You have successfully replaced the mock login with real user authentication using **Amazon Cognito**.  
The **Amplify UI Authenticator** allows for quick, secure, and maintainable implementation.  
The **Pinia Store** has been updated to track the real user login state.  

Next: Migrate all mock data currently stored locally to the cloud using a real backend service.
