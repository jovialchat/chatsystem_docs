# Architectural Overview
In jovial Chat System we follow a scalable system of microservices. 

## Components involved

**Micro Services**

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

