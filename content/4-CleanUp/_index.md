---
title : "Conclusion and Resource Cleanup"
date : "2025-06-14"
weight : 4 
chapter : false
pre : " <b> 4. </b> "
---

After completing the **"Building an Event Ticket Booking Web App with AWS Amplify"** workshop, you now have a solid understanding of how to build a **serverless full-stack application**, from implementing user authentication using **Amazon Cognito** to storing data with **Amazon DynamoDB**.

---

### Summary of Key Learnings

- Installed and configured a Vue.js frontend application  
- Added backend authentication service using Cognito via Amplify CLI  
- Designed data schema with GraphQL  
- Automatically generated database tables in DynamoDB  
- Connected frontend to real backend using GraphQL API  
- Handled user event ticket bookings and data synchronization  

The serverless approach enables you to build applications that are **flexible – cost-effective – scalable**, paying only for what you use.

---

### Clean Up Unused Resources

If you used a personal AWS account, **you should delete all created resources after the workshop** to avoid unnecessary ongoing charges.  
To remove backend resources created during the workshop, you can use the Amplify CLI.  
Make sure Amplify CLI is installed and you are logged in to your AWS account.  
In the root folder of your project, run:

```bash
amplify delete
```
This process will:  
- Delete all backend resources on AWS (Cognito, DynamoDB, AppSync, etc.)  
- Also remove local Amplify configuration files such as `schema.graphql`, the `amplify/` folder, etc.  

{{% notice warning %}}
If you want to keep your schema or config, make sure to back them up before deleting.
{{% /notice %}}

- The deletion may take a few minutes depending on the number of resources.  
- You will be prompted for confirmation — type `Y` to continue.  
- When completed successfully, you will see:

```bash
√ Project deleted in the cloud.
✅ Project deleted locally.
```
![DB](/images/3.connect/03-DB3.png)
{{% notice warning %}}
If you DO NOT delete the Amplify project, resources like DynamoDB or Cognito will continue running on your AWS account and may incur unnecessary costs!
{{% /notice %}}

---

### ✅ Workshop Completed!

Congratulations on completing the full workshop!  
We hope this experience inspires you to explore more **serverless solutions** from AWS, such as:

- Amazon S3 – for storing event images  
- AWS Lambda – for handling backend logic  
- Amazon API Gateway – for building REST APIs  
- Amazon AppSync (Advanced) – for real-time and multi-source GraphQL APIs  

**See you in the next workshop — and best of luck with your AWS journey!**

