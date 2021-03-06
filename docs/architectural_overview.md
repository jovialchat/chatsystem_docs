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

 **A client must have the following attributes**
 
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

File Store System microservice is built to store and read media files.
FSS ensures secured access to the files using **`file_token`s**. 

All files in FSS are stored in a Masterless DB as a key-value store. Where the key is uniquely assigned `file_id` and value is the `file_data`.

`file_token` is a web token comprising of following fields **`file_id`, `access` & `mime_type`** signed using a secret key.

 **FSS must allow following operations**

 - Upload File and store in masterless DB
 - Create `file_token` (file access token) 
 - Read file using `file_token` , where `user_token` is also verifyed and checked if the user has access to a perticular file.


### Message Delivery System (MDS)

Message Delivery Systems is a hardware and network failure resistant system to deliver messages as per the flow of **Message Delivery lifecycle**, starting from sender's device to reciver's device via the MDS backend services.

 **MSD must allow following operations**

 - Send Message
 - Read recived messages
 - Read sent messages

### Message Storage System (MSS)

Message Storage System is component operated by MDS, to store message locally client devices and also online in Master Less DB.

 **MSS must allow following operations**

 - Store and read messages in **client device**
 - Store and read messages from **Master Less DB**.


## Backend Components

### UMS server

**User Management System Server** is an operational server of the UMS. The ports of UMS server support REST API call to allow the user to access the system's UMS.

### MDS server

**Message Delivery System Server** is an operational server of the MDS. The user can connect to the MDS port with WebSocket or raw TCP connection with MDS Server.

### FSS server

**File Store System Server** is an operational server of the FSS. The user can store and retrieve files in FSS using Rest API calls conventionally and also use WebSocket or raw TCP depending on the use-case of a file & devices.

### MongoDB Document Database

**MongoDB** is a NoSQL Document Database. A MongoDB instance can have multiple databases, a database has multiple collections, and collection stores multiple documents indexed as a B+ Tree.
MongoDB supports shredding in a strict Master-Slave architecture and guarantees ACID-compliant operation at the level of a single document.

Here we store some essential user data and most important message receiving index. `message_index` is used to ensure serial unique identification of message which are stored in the table of a Masterless Database.

### CassandraDB Masterless Database


## Frontend Components

### MDS client
**Message Delivery System Client** queues messages locally and sends them to the MDS server in presence of the network. All client Devices will have MDS as per their respective environments.

### Local MSS
**Local Message Storage System** stores messages sent and received by the client on the client's device. The messages are stored with a local index and receiver's message index.

### Local Triggers
### Client Application
