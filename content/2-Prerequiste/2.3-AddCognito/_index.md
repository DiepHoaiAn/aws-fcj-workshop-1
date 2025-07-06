---
title : "Add Amazon Cognito Service"
date : "2025-06-14" 
weight : 3 
chapter : false
pre : " <b> 2.3 </b> "
---

After understanding the UI and mock login logic of the MUSSEL application, we will now start integrating **real user authentication** using **Amazon Cognito**, through **AWS Amplify**.

---

### Objective

- Add real login system (Amazon Cognito) to the MUSSEL app.
- Use the **Amplify CLI** to create, configure, and deploy the backend.
- Pull the backend to local machine for frontend integration.

---

### Steps to Follow

#### 1. Initialize Amplify Project

Open the terminal inside the `my-event-planner-sample-app` folder and run:

```bash
amplify init
```
Select the following configurations:
| Question                      | Answer                    |
| ----------------------------- | ------------------------- |
| Name                          | eventplanner              |
| Environment                   | dev                       |
| Default editor                | Visual Studio Code        |
| App type                      | javascript                |
| JavaScript framework          | vue                       |
| Source Directory Path         | src                       |
| Distribution Directory Path   | dist                      |
| Build Command                 | npm run-script build      |
| Start Command                 | npm run-script serve      |
| Initialize project config     | Yes                       |
| Authentication method         | AWS access keys           |
| accessKeyId / secretAccessKey | (enter from AWS IAM user) |

After a few minutes, you should see:
```
✔ Initialized your environment successfully.
✔ Your project has been successfully initialized and connected to the cloud!
```
#### 2. Add Authentication Service (Cognito)

Run the command: `amplify add auth`  
Choose the following options:

| Question                                                         | Answer     |    
|------------------------------------------------------------------|------------|
| Do you want to use the default authentication and security configuration? | Yes        |
| How do you want users to be able to sign in?                     | Username   |
| Do you want to configure advanced settings?                      | No         |    

After completion, you should see:

```plaintext
✔ Successfully added auth resource eventplannerAuth
```
#### 3. Deploy backend to AWS
Run the command: `amplify push`
Select `Yes` to confirm the creation of AWS resources. This process will take around 1–2 minutes.
You should see:
```plaintext
✔ All resources are updated in the cloud.
```
#### 4. Pull backend to local
Run the command: `amplify pull`
Choose the following options:
| Question                                                         | Answer |
| ---------------------------------------------------------------- | ------ |
| Do you want to use an existing environment?                      | Yes    |
| Select the environment you want to use                           | dev    |
| Do you want to generate code for your newly created GraphQL API? | Yes    |  

After completion, you should see:
```plaintext
✔ Successfully pulled backend environment dev from the cloud.
```
#### 5. Install Amplify Libraries
Run the following command to install necessary libraries:
```bash
npm install aws-amplify @aws-amplify/ui-vue
```
#### 6. Update Amplify Configuration in the App
Open the file `src/main.js` and add the following code to configure Amplify:

```javascript
import Amplify from 'aws-amplify';
import awsExports from './aws-exports';
Amplify.configure(awsExports);
```

7. Restart the Application
Run the following command to restart the app:
```bash
npm run dev
```  

Open your browser and go to `http://localhost:3000/` to check.  
You should now see the updated login interface with real authentication functionality.  
At this point, you can sign in using a username that was created in Amazon Cognito.

---

### Result

You have successfully integrated **Amazon Cognito** into the MUSSEL application.  
Users can now log in and use the app with real authentication.  
In the upcoming steps, you will continue building a real backend using **Amazon DynamoDB** and **GraphQL API**, connecting them to the frontend to complete the application.

{{% notice tip %}}
Make sure to double-check each step and configuration to avoid deployment errors.
{{% /notice %}}

---

### Conclusion

You have completed adding a user authentication service to the MUSSEL app using **Amazon Cognito**.  
In the next steps, you'll continue adding the necessary code to connect the app to a real backend to enable login and user data management.  
Be sure to carefully check your steps and configurations to prevent errors during deployment.

{{% notice tip %}}
Make sure to delete the Amplify app and all associated resources after completing the workshop to avoid unnecessary costs.
{{% /notice %}}

{{% notice warning %}}
If you encounter errors during configuration or deployment, review your steps and ensure all required libraries have been properly installed.  
If needed, you can consult the official [AWS Amplify Documentation](https://docs.amplify.aws/) for detailed instructions on commands and setup.
{{% /notice %}}
