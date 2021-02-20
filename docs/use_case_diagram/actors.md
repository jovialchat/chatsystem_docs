# Actors in MessageSystem UseCase

## Client

The client is any physical entity that can send or receive messages using the system.
A client participates in the Message Delivery Process, using a **Client Device** or **A set Client Devices** to interact with ***Message Delivery System***.

### Client Device

A Client Device has the following operations:-
- **`queueing`**: All newly created messages are stored in a queue, then sent in the order in which they were born.
- **`sending`**: The message stored in the queue are sent, by requesting the MDS to annotate, index, and store the message.
- **`receive`**: The Client devices look for needed messages sent to the client, this process takes place in 2 steps:-
  - `look`: Seeking the index number of the latest message sent to the Client.
  - `fetch`: Limited number of unreceived messages are fetched from MDS ports.
- **`local storage`**: All messages sent & received by the client device are stored locally.
