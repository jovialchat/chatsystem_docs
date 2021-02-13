# Architectural Overview
In jovial Chat System we follow a scalable system of microservices. 

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

**Client Side Components**

 - MDS client
 - Local MSS
 - Local Triggers
 - Clinet Application
 

## Client
Client is any physical entity which can send or recive messages using the system.

A client must have follow attributes:-
- **`app_id`** unqiue id of the user's parent app
- **`user_id`**: uniquely assigned to the user by the chat system and is immutable.
- **`username`** is an unique *string* assigned manually by the client during registration. **(optional)**
- **`password`** is assigned manually by the client during registration.


## Services

### User Management System (UMS)

User Management System mircoserive is required for client/user registration, authorization and verification.

Jovial Chat System requires secured client identification and verification. To fullfill this UMS allows the client aquire a web token signed with a secret key, this signed token can be used to access other micoservices (such as FSS & MDS).

### File Store System (FSS)
### Message Delivery System (MDS)
### Message Storage System (MSS)

