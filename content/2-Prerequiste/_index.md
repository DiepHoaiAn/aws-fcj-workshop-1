---
title : "Authenticating a Web App with AWS Amplify"
date : "2025-06-14"
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---

### Objective

This lab helps you launch the sample MUSSEL frontend application and set up authentication (**AuthN** & **AuthZ**) using **AWS Amplify + Amazon Cognito**.

Before you begin, make sure you have installed the required tools as described in **1.2 Environment Setup and Tool Installation**.

---

### MUSSEL App Authentication Architecture

The diagram below illustrates how the frontend (running at localhost:3000) interacts with Amazon Cognito authentication services through AWS Amplify:

![Authentication flow of MUSSEL with AWS Amplify and Cognito](/images/2.prerequisite/02-Frontend-Authentication-Flow-with-AWS.png)

- **Amplify CLI**: used to initialize (`init`) and deploy (`deploy`) the backend to AWS.
- **Vue.js App**: the UI is preconfigured with the **Amplify Auth** library to send authentication requests.
- **Amazon Cognito**: handles login and signup requests, then returns authentication tokens.
- **Returned Tokens**: are used to authenticate other APIs (such as data queries or ticket booking).

{{% notice info %}}
This chapter only covers the authentication (Auth) part – it does not yet include data storage (GraphQL or DynamoDB).
{{% /notice %}}

---

#### Step 1: Clone the Sample App from GitHub

1. Open **Visual Studio Code**.

2. Click the **Source Control** icon (branch icon on the left sidebar).

3. Click **Clone Repository**.

4. When the **Command Palette** appears, paste the following URL:  
https://github.com/build-on-aws/my-event-planner-sample-app.git

5. Choose a local folder to save the repo (e.g., `C:\code` or `~/Projects`).

{{% notice tip %}}
Avoid using cloud-synced folders like Dropbox or OneDrive — they may cause issues when running the app.
{{% /notice %}}

6. When prompted: **"Would you like to open the cloned repository?"** → choose **Yes**.

7. If you see a popup **"Do you trust the authors..."**, tick **"Yes, I trust the authors"**.

---

#### Step 2: Select the Correct Branch

1. Look at the lower left corner of the **Status Bar** and click on the branch name (usually `main`).

2. Choose the branch: `origin/lab1-start`

Once selected, VS Code will show `lab1-start` as displayed below:

![Select lab1-start branch](/images/2.prerequisite/01-branch-lab1-start.png)

---

#### Step 3: Install Dependencies

1. Open the **Command Palette** using the shortcut:
- **Windows**: `Ctrl + Shift + P`
- **Mac**: `Cmd + Shift + P`

2. Type: `> Tasks: Run Task` → press `Enter`.

3. Choose: `npm` → `npm: install`  
> This step will download and install all frontend dependencies.

---

#### Step 4: Run the Application

1. Repeat the action: `Tasks: Run Task`.

2. Choose: `npm` → `npm: dev`

3. The terminal will display something like this:
```
VITE v3.2.8 ready in 522 ms

➜ Local: http://localhost:3000/
➜ Network: http://172.31.38.233:3000/
Press h to show help
```

4. Open your browser and visit `http://localhost:3000/` to preview the app interface.

{{< figure src="/images/2.prerequisite/mussel-local-preview.png" >}}

---

#### Step 5: Stop the Application

- In the **Terminal** window, click the 🗑 **Kill Terminal** icon in the top-right corner.
- You may leave the terminal running and edit the code — the site will auto-refresh if using hot-reload.  
- Explore the code by clicking **Explorer** (folder icon on the left in VS Code).  
- If needed, open a new terminal by clicking the `+` icon in the top-right of the terminal panel.

{{% notice warning %}}
If you don't stop it properly, running `npm: dev` again may start multiple instances, which can cause conflicts or slow down your machine.
{{% /notice %}}

### Conclusion

You have successfully **installed and launched the sample MUSSEL frontend application** in local mode.  
The next steps will guide you through integrating **AWS Amplify** to add real backend functionality.

### Next Steps
- [2.1 Explore the MUSSEL App](./2.1-MUSSELapp/)
- [2.2 Explore the MUSSEL Codebase](./2.2-MUSSELrepo/)
- [2.3 Add Amazon Cognito Service](./2.3-AddCognito/)
- [2.4 Integrate Real Login with Cognito](./2.4-LogIn/)