---
title : "Fetching Data with Amazon DynamoDB"
date : "2025-06-14"
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---

After successfully implementing user authentication with **Amazon Cognito**, your team at **FCJ-UTE Computing** now requests the next step:

> Migrate all mock data currently stored locally to the cloud using a real backend service.

The requirement is to use **AWS Amplify** together with **Amazon DynamoDB** to store the data.

---

### Functional Objectives

Users of the MUSSEL application should be able to:

- View lists of upcoming or past events.
- See the event locations.
- Know how many tickets are available.
- Check which events they’ve registered for.

Currently, all this data is still **mocked** and stored temporarily in local memory. We need to move it to the cloud.

---

### Database and NoSQL

To store dynamic data, we need a database service. In serverless architecture, a common choice is:

- **Amazon DynamoDB** – a fully managed NoSQL database.
- Supports standard **CRUD** operations (Create, Read, Update, Delete).
- Easily integrates with **AWS Amplify** and GraphQL APIs.

---

### Data Model (Entities & Attributes)

We will define the following entities:

#### Event

- *id*: Event ID (required)
- *name*: Event name
- *description*: Description
- *event_owner*: Organizer
- *room_id*: Room ID
- *image_file_name*: Image file name
- *event_datetime_start*, *event_datetime_end*
- *event_duration*, *total_tickets*
- *tickets*: Array of *user_id* who booked

#### Room

- *id*: Room ID
- *name*: Room name
- *capacity*: Capacity
- *bookings*: List of previous bookings (*datetime_start*, *datetime_end*)

#### User (implicitly handled via Cognito)

- *user_id*: User ID (same as Cognito sub)
- User data will not be stored directly but referenced inside *tickets[]* of the Event entity.

{{% notice tip %}}
We won’t create a dedicated User table, since user information is handled by Cognito.
{{% /notice %}}

---

### Switching the Data Access Flow

Currently, the app uses the following logic:

```ts
dataStore.initMockData(); // generate mock data
```
We are about to replace this with actual GraphQL queries to fetch data from DynamoDB:  
```ts
const rooms = await apiClient.graphql({ query: listRooms });
const events = await apiClient.graphql({ query: listEvents });

this.rooms = rooms.data.listRooms.items;
this.events = events.data.listEvents.items;
```
### Architecture Diagram – Connecting to DynamoDB

![arch-diagram](/images/3.connect/System_Architecture.png)

{{% notice tip %}}
- The app sends GraphQL queries/mutations to AppSync.
- AppSync reads/writes data from/to DynamoDB.
- Cognito validates authentication tokens when needed.
{{% /notice %}}

{{% notice info %}}
This is the core data flow that allows the frontend to interact with real cloud data instead of mock data.
{{% /notice %}}

### Updating Data (e.g., Booking Tickets)
Previously, booking a ticket was done via local memory: `event.bookTicket(userId);`  
Now, we need to sync this change to the cloud:
```ts
const input = {
  id: event.id,
  tickets: [...event.tickets, userId]
};

await apiClient.graphql({
  query: updateEvent,
  variables: { input }
});
```

{{% notice warning %}}
Only authenticated users are allowed to perform this mutation.
{{% /notice %}}

### Conclusion
You’ve learned why and how to migrate from mock data to a real backend system.  
DynamoDB will replace local data and become the main data source.  
All operations (queries and mutations) will now be handled through the GraphQL API.  
These operations will respect the authorization logic powered by Amazon Cognito.  

Next: You will define these data entities in the GraphQL schema, deploy to AWS, and integrate into the frontend.
  
### What’s next
- [3.1 Modeling Data with AWS Amplify and GraphQL](./3.1-ModellingEntities/)
- [3.2 Adding GraphQL Queries to Connect Real Data](./3.2-AddQueries/)
