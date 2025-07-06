---
title : "Environment Setup and Tool Installation"
date : "2025-06-14"
weight : 2 
chapter : false
pre : " <b> 1.2 </b> "
---

### Device and Account Preparation

1. You’ll need a device running **Windows**, **macOS**, or **Linux**.  
   + The OS doesn't matter as long as the tools below are functional.

2. AWS Account:
   + If you're attending a **hosted workshop**, you’ll be provided with a temporary AWS account.
   + If you're **self-learning**, you'll need to create and use your own AWS account.

{{% notice warning %}}
This workshop will provision AWS services that **may incur charges**.  
**Be sure to delete the Amplify app and all resources once you've completed the workshop.**
{{% /notice %}}

---

### Required Software Installation

1. Install **Node.js** (includes NPM):  
   + Download from: [https://nodejs.org/en/download/](https://nodejs.org/en/download/)

2. Install **AWS Amplify CLI** using the command:
   ```bash
   npm install -g @aws-amplify/cli
   ```
3. Install TypeScript (for local dev):
    ```bash
   npm install -g typescript
   ```
4. Install Git:  
+ Download from: https://git-scm.com/downloads

5. Install Visual Studio Code:  
+ Download from: https://code.visualstudio.com/download

---

### Check Software Versions
After installation, verify the versions to ensure they are correct or up to date:  

```bash
node -v
npm -v
amplify -v
git -v
code -v
npm ls typescript
```
You will receive output versions similar to the following (or newer):  
![Check_version](/images/1.introduction/check_version.png)

---

### Configure IAM Access Keys for Amplify CLI

1. Go to **AWS Console** → **IAM** → **Users** → **Create user**.

2. Name the user: `amplify-dev` (or any name you prefer).

3. Assign the following permissions to the user:
   + `AdministratorAccess-Amplify`
   + `AmplifyBackendDeployFullAccess`

4. Create Access Key:
   + Choose the "Programmatic access" method  
   + Download and store the Access Key ID & Secret Access Key securely

{{% notice info %}}
During the workshop, you'll need to use these credentials when running `amplify init`.  
If you're participating in an AWS-hosted workshop, these credentials may already be provided.  
**Make sure not to share your Access Keys with others to avoid security risks.**
{{% /notice %}}

{{% notice tip %}}
If you're unfamiliar with Access Key management, refer to the official AWS documentation for best practices on securing and using them.  
If you encounter errors during installation or setup, consult the documentation or ask the community (e.g., AWS Study Group).
{{% /notice %}}

---

### References
- [AWS Amplify CLI Documentation](https://docs.amplify.aws/cli/)
- [Node.js Documentation](https://nodejs.org/en/docs/)
- [Visual Studio Code Documentation](https://code.visualstudio.com/docs)
- [Git Documentation](https://git-scm.com/doc)
- [AWS IAM Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)