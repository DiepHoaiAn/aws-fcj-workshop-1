---
title : "Explore the MUSSEL App Source Code"
date : "2025-06-14" 
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---

The goal of this section is to help you understand the source code structure and the mock login mechanism of the MUSSEL application before integrating Cognito.

---

### Main Folder Structure

| Component              | Main Functionality                                  |
|------------------------|-----------------------------------------------------|
| public/               | Stores event illustration images                    |
| index.html            | Entry point of the application                      |
| src/main.ts           | Initializes the Vue app and loads mock data         |
| src/App.vue           | Root component of the UI                            |
| src/components/       | UI elements: AppBar, BottomNav, EventsList, etc.    |
| src/store/            | Manages state (auth, data, UI, mock data)           |
| src/models/           | Data models (Event, Room)                           |
| src/plugins/          | Vuetify, WebFont, and other plugin configurations   |

---

### Main Initialization Flow

- **index.html** includes **main.ts**
- **main.ts** calls **dataStore.initMockData()** to generate mock data
- Then it loads **App.vue** to render the application layout

### How the Sign In Button Works

#### File AppBar.vue

```vue
<v-btn v-if="userSignedIn" @click="showSignOut">Sign Out</v-btn>
<v-btn v-else @click="showSignIn">Sign In</v-btn>
<SignInOutDialogs />
```
Based on the `userSignedIn` variable from `authStore.userAuthenticated`.

When **Sign In** is clicked → calls **showSignIn()** → opens the dialog.

#### File SignInOutDialogs.vue  

Displays a `v-dialog` when `showDialog = true`. It doesn't call any API, just assigns:
```ts
function signIn() {
  authStore.userAuthenticated = true;
}
```
→ Simulates the signed-in state.

{{% notice info %}}
The Sign In functionality at this stage is purely mock.
You will replace it with real authentication using Amazon Cognito in the next steps.
{{% /notice %}}

### Conclusion
- The source code is clearly organized using a component-based structure.  
- Uses Vue 3 + Pinia for state management.  
- The current sign-in logic is not connected to a backend – only simulates login on the UI.  

Next: Start implementing real authentication by integrating Amazon Cognito with Amplify.