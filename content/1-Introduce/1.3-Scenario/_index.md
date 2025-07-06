---
title : "Scenario: The MUSSEL Event App"
date : "2025-06-14"
weight : 3 
chapter : false
pre : " <b> 1.3 </b> "
---

### Scenario Introduction

Congratulations! You’ve just been hired at **FCJ-UTE Computing** as a software engineer!

In this project, you’ll be working on a web application called **MUSSEL** (My University’s Sports & Social Events List) – a platform that helps students easily browse and reserve tickets for university events.

---

### Application Description

- MUSSEL is a **sample web application** built by the **AWS Workshop Studio team**, designed to help learners gain hands-on experience deploying modern cloud applications using **AWS Amplify**.
- The app is provided as an **incomplete prototype**, which initially runs only in local mode with mock data.
- Your role as a newly onboarded software engineer is to **integrate AWS cloud services to enable real backend functionality**.

---

### Lab Objective

{{% notice info %}}
This workshop **does not focus on teaching frontend development from scratch**.  
The entire frontend source code is already provided by the AWS Studio development team.
{{% /notice %}}

Instead, the lab emphasizes hands-on experience in integrating and connecting AWS services with the provided frontend through steps such as:

- Initializing a cloud environment using **AWS Amplify**
- Adding **user authentication** with **Amazon Cognito**
- Creating a **dynamic database** using **Amazon DynamoDB + GraphQL API**
- Connecting the provided frontend to real backend services via actual queries and mutations

{{% notice tip %}}
Through this process, students will gain exposure to the complete deployment flow of a real-world serverless application.  
It lays the foundation for essential skills in today’s cloud-native software development landscape.
{{% /notice %}}

---

### MUSSEL App Interface (Initial – Mock Data)

![MUSSEL App Interface](/images/1.introduction/scenario-app-preview.png)

The interface displays a list of sample events with images, detailed information, ticket status, and more.  
These data items are randomly generated from a mock data script when running the app in `npm run dev` mode.

---

#### Initial State

- The app only runs locally.
- No real login feature.
- No data is persisted – all actions are lost on page refresh.

---

#### Your Assigned Tasks

You will be responsible for the following:

1. Initialize an Amplify project for the existing frontend.
2. Add user authentication (Auth) using Amazon Cognito.
3. Design a data model (GraphQL schema) for entities like *Event*, *Room*, *Booking*.
4. Integrate and sync real backend data using DynamoDB.
5. Connect all frontend actions (sign in, ticket booking, mock data creation) to actual backend APIs.

#### Workshop Workflow Diagram

This diagram illustrates the steps you will follow in this workshop:

![Workflow - Amplify Integration](/images/1.introduction/workflow-overview.png)

> The diagram outlines the full journey from setting up the local environment, configuring backend services with AWS Amplify, to integrating the frontend with the real backend.  
The final result is a full-stack MUSSEL application running reliably with real data on the cloud.

Key steps in the workflow:

1. Clone the sample MUSSEL app from GitHub and test it locally.
2. Initialize an AWS Amplify environment for the frontend project.
3. Add user authentication service using Amazon Cognito.
4. Design the GraphQL data schema (Event, Room, Booking).
5. Automatically generate the backend using DynamoDB and GraphQL API (`amplify push`).
6. Connect the existing frontend to the backend API via mutations/queries.
7. Fully test the user flow: sign in, book tickets, persist data.
8. Full-stack serverless application completed*.

{{% notice info %}}
This app is a sample product developed by the AWS Studio team, with a ready-made frontend provided for training purposes.  
However, the entire process of cloud integration, service configuration, deployment, and testing must be performed manually by the learner throughout the workshop.
{{% /notice %}}