---
title: "Introduction"
date: "2025-06-14"
weight: 1
chapter: false
pre: "<b> 1. </b>"
---

### Overview

This chapter provides a comprehensive overview of the goals, required skills, and real-world scenario simulated throughout the **"Building an Event Ticketing Web App with AWS Amplify"** workshop. It serves as a crucial foundation, helping you understand the direction, core technologies, and what you will achieve by the end of the workshop.

This workshop is not only designed to strengthen your hands-on experience with **AWS Amplify**, but also to help you understand how to build a practical application — with user authentication, backend data queries, and cloud-native data storage via GraphQL.

---

### From Real-World Needs to an AWS Solution

In many organizations — especially universities or event organizers — managing events and tickets is often done manually or using Excel, which makes it hard to maintain or scale.

This workshop simulates the requirement of building a system that:

- Allows users to **log in, view events, and book tickets**
- Stores all data **centrally in the cloud**, ensuring availability and security
- Provides a user-friendly interface that **runs entirely in the browser** (SPA – Single Page Application)

---

### Overall Architecture Diagram

To get a clear picture of the full system you’ll build in this workshop, let’s take a look at the following architecture diagram:

![System architecture for the event ticketing web app](/images/1.introduction/01-Architecture.png)

#### Diagram Explained:

- **User (Browser):** accesses the application via browser over HTTP
- **Vue.js Web App:** the frontend built with Vue.js running at `localhost:3000`, responsible for UI, login, and sending data queries
- **Amplify Auth Library:** handles user login and authentication with Amazon Cognito
- **Amplify GraphQL Client:** sends queries and mutations to AWS AppSync with an authentication token (if logged in)
- **Amazon Cognito:** authenticates users and returns a JWT token for backend access verification
- **AWS AppSync:** a GraphQL API service that mediates communication between the frontend and DynamoDB
- **Amazon DynamoDB:** stores actual data such as the *Room Table* and *Event Table*

#### Basic System Flow:

- The user opens the browser and accesses the app at `localhost:3000`
- The Vue.js frontend renders the UI and waits for the user to log in
- When login is attempted, the **Amplify Auth Library** sends a sign-in request to **Amazon Cognito**
- If successful, Cognito returns a **JWT auth token**
- This token is attached to all subsequent queries or mutations sent via the **Amplify GraphQL Client**
- **AppSync** receives the query, verifies the token, and interacts with **DynamoDB** accordingly
- Data (e.g., event list, booking info) is returned and displayed to the user in the frontend

#### Architecture Highlights:

- **Fully serverless:** No servers to manage — everything runs on AWS managed services
- **Frontend-backend separation:** Frontend only communicates through API; it doesn't access the database directly
- **Secure by design:** Only authenticated users (via Cognito) can perform data updates
- **Highly scalable:** The system can easily scale to support more users in the future

{{% notice info %}}
All backend services are fully managed by AWS — you don’t need to build your own servers or handle scaling manually.
{{% /notice %}}

---

### Real-World Scenario & Solutions

| Real-World Challenge | AWS-Based Solution |
|----------------------|-------------------------|
| Users must log in to book tickets | Use **Amazon Cognito** for user authentication |
| Admin wants centralized cloud-based data | Use **DynamoDB** for `Room` and `Event` tables with CRUD operations |
| Frontend needs simple and fast interaction with backend | Use **AppSync (GraphQL API)** for efficient data queries and mutations |
| No backend/devops team available | Use **Amplify CLI + Hosting** to deploy everything with a single command |

---

### Why Use Amplify?

- **Optimized for frontend developers:** No backend coding required, just JavaScript
- **Seamless integration** with Cognito, AppSync, and DynamoDB
- **Friendly CLI:** Deploy, update, and remove backend resources with a single command
- **Auto-generated code** for GraphQL queries and mutations to use directly in the frontend

{{% notice tip %}}
The entire architecture is based on a serverless model — helping you reduce cost, simplify deployment, and eliminate backend maintenance concerns.
{{% /notice %}}

---

### Conclusion

Chapter 1 helps you understand the **goals and context** of the workshop, while also providing a clear picture of the **AWS infrastructure** used. The following chapters will walk you through each component in detail: from setting up user authentication, to fetching real data, and finally to updating and syncing ticket information.

---

### What’s Next

- [1.1 Learning Objectives](./1.1-LearningObjectives/)
- [1.2 Technical Requirements & Skills](./1.2-Prerequisites/)
- [1.3 Simulated Scenario](./1.3-Scenario/)
