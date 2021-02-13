# Architectural Overview
In jovial Chat System, we follow a scalable system of microservices. 

## Components involved

**Services**

 - User Management System (UMS)
 - File Store System (FSS)
 - Message Delivery System (MDS)
 - Message Storage System (MSS)

**Backend Components**

 - UMS server
 - MDS server
 - FSS server
 - MongoDB Document Database
 - CassandraDB Masterless Database

**Client-Side Components**

 - MDS client
 - Local MSS
 - Local Triggers
 - client Application
 
## App
App is an application registered to the Jovial Chat System. All apps have a unique **`app_id`** and a random **`app_secret`**.
**`app_key`** is web token prepared using `app_id` and `app_secret`, signed with a secret key.

## Client
Client is any physical entity which can send or receive messages using the system.

A client must have the following attributes:-
- **`app_id`** unique id of the user's parent app
- **`user_id`**: uniquely assigned to the user by the chat system and is immutable.
- **`username`** is a unique *string* assigned manually by the client during registration. **(optional)**
- **`password`** is assigned manually by the client during registration.

## Services

### User Management System (UMS)

User Management System microservice is required for registration, authorization and verification of clients and applications

Jovial Chat System requires secured client identification and verification. To fulfil this UMS allows the client to acquire a web token signed with a secret key, this signed token can be used to access other microservices (such as FSS & MDS).

**UMS must allow following operations**

- Register new Apps to the system and provide app_key
- Verify App with app_key
- Register new user to the system
- User login and provide signed user token

### File Store System (FSS)
### Message Delivery System (MDS)
### Message Storage System (MSS)

