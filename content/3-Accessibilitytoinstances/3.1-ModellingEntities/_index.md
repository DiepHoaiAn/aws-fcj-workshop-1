---
title : "Modeling Data with AWS Amplify and GraphQL"
date : "2025-06-14"
weight : 1 
chapter : false
pre : " <b> 3.1. </b> "
---

After setting up the environment, we will define the data models (entities) and connect them to a real database using **AWS Amplify** and **GraphQL**. This is the first step in storing events, rooms, and tickets in **Amazon DynamoDB**.

---

### Objectives

- Transition from mock data to real persistent storage.
- Create a GraphQL schema with the entities: `Room`, `Event`, `Booking`.
- Use Amplify CLI to generate DynamoDB tables and corresponding code.

---

### Step-by-step Instructions

#### Step 1: Switch to the *lab2-start* branch
Open the terminal and navigate to your project folder. Then switch to the `lab2-start` branch:

```bash
git checkout lab2-start
```
Alternatively, you can use the Git UI in VS Code to switch branches.  
You’ll find the branch list in the lower left corner of VS Code.  
![Change branch](/images/3.connect/01-branch.png)

{{% notice warning %}}
If you haven't cloned the repo yet, please follow the instructions in 1.2 Environment Setup and Tool Installation.
{{% /notice %}}

{{% notice info %}}
The **lab2-start** branch contains all results from Lab 1 and is ready for Lab 2.
{{% /notice %}}

Then run `npm install` to install required packages.

#### Step 2: Add API service with GraphQL

In the root directory of your project, run: `amplify add api`  
Select the following configuration when prompted:

| Question                                         | Answer                    |
| ------------------------------------------------ | ------------------------- |
| Select from one of the below mentioned services  | GraphQL                   |
| Authorization modes                              | API key (default)         |
| API key description                              | Read Access               |
| Expiration (days)                                | 7                         |
| Configure additional auth types?                 | Yes                       |
| Choose additional authorization types            | Amazon Cognito User Pool  |
| Use Cognito user pool configured in project?     | Yes                       |
| Choose a schema template                         | Single object with fields |
| Do you want to edit the schema now?              | Yes                       |

#### Step 3: Define schema for the entities

Amplify will open the file at `amplify/backend/api/.../schema.graphql`.  
If it doesn’t open automatically, you can search for the file by name.  
Delete all existing content (such as `type Todo`) and replace it with:
```graphql
input AMPLIFY { globalAuthRule: AuthRule = { allow: public } } # FOR TESTING ONLY!

type Booking {
  datetime_start: AWSDateTime!
  datetime_end: AWSDateTime!
}

type Room @model
  @auth(rules: [{ allow: public, operations: [create, read, delete] }]) {
  id: ID!
  name: String!
  capacity: Int!
  bookings: [Booking]!
}

type Event @model
  @auth(rules: [{ allow: public, operations: [create, read, update, delete] }]) {
  id: ID!
  name: String!
  description: String!
  event_owner: String!
  room_id: String!
  image_file_name: String!
  event_datetime_start: AWSDateTime!
  event_datetime_end: AWSDateTime!
  event_duration: Int!
  total_tickets: Int!
  tickets: [String!]!
}
```
{{% notice warning %}}
This is a demo lab, so all data is public – do not use in production environments!
{{% /notice %}}

#### Step 4: Deploy schema to AWS

After saving the **schema.graphql** file, push the schema to the cloud with: `amplify push --y` 

This process may take a few minutes. Amplify will:  
- Create DynamoDB tables for Room and Event.  
- Auto-generate GraphQL CRUD code in the src/graphql folder.  

### Result after this step  
- Data entities are now defined and deployed to DynamoDB.  
- CRUD source code (queries, mutations) is ready for frontend integration.  
- The application now has a working backend for managing events and rooms.  

{{% notice tip %}}
You can view the created tables in the AWS Console under DynamoDB → Tables.
The table names typically begin with eventplanner-<env>-Room or ...-Event.
{{% /notice %}}

{{% notice info %}}
DynamoDB tables can be viewed in the AWS Console → DynamoDB → Tables.
Table names usually start with eventplanner-<env>-Room or ...-Event.
{{% /notice %}}

Next up: Connect the frontend to the real backend by using the auto-generated GraphQL queries and mutations.
