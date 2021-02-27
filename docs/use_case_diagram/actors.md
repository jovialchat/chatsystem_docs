# Actors in MessageSystem UseCase

## Client

The client is any physical entity that can send or receive messages using the system.
A client participates in the Message Delivery Process, using a **Client Device** or **A set Client Devices** to interact with ***Message Delivery System***.

### Client Device

Client Device is a physical entity of **Client**.

A Client Device has the following operations:-
- **`queueing`**: All newly created messages are stored in a queue, then sent in the order in which they were born.
- **`sending`**: The message stored in the queue are sent, by requesting the MDS to annotate, index, and store the message.
- **`receive`**: The Client devices look for needed messages sent to the client, this process takes place in 2 steps:-
  - `look`: Seeking the index number of the latest message sent to the Client.
  - `fetch`: Limited number of unreceived messages are fetched from MDS ports.
- **`local storage`**: All messages sent & received by the client device are stored locally.

### Set of Client Device

A User in a Chat system can utilize multiple devices as a set of Devices.
This setup is for daemons that require load balancing to sustain messages from a large number of senders.

### Client Device Load Balancing

For Load Balancing on Device Set, we follow Round Robin WRT connection initiation.

For example:-

##### Devices assigned

```js
//Client Set with client_id: 'abcd88u21noinoin3332'
client_device1 = {
   client_id: "abcd88u21noinoin3332",
   device_id: "sbcd8noinoin33328u21"
}
client_device2 = {
   client_id: "abcd88u21noinoin3332",
   device_id: "sbcd8noinoin33328u22"
}
client_device3 = {
   client_id: "abcd88u21noinoin3332",
   device_id: "sbcd8noinoin33324u00"
}

//Client Device set represented on client document
client = {
   client_id: "abcd88u21noinoin3332",
   devices: [
      { device_id: "sbcd8noinoin33328u21", state_conncted: true, set_sub_factors: [0] },
      { device_id: "sbcd8noinoin33328u22", state_conncted: true, set_sub_factors: [1] },
      { device_id: "sbcd8noinoin33324u00", state_conncted: true, set_sub_factors: [2] },
   ]
}
```

##### Messages sent to the `User`

| message_index | reciver_id | sender_id | data |
|--|--|--|--|
| 1 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...400>` |
| 2 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...430>` |
| 3 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...100>` |
| 4 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...230>` |
| 5 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...550>` |
| 6 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...430>` |

**sender_id**: UserId of message sender
**reciver_id**: UserId of message reciver
**message_index**: serial number of message sent to a reciver
**Primary Key**: (`message_index`+`reciver_id`)

##### Message Received by any client_device

```js
set_device_count //total number of devices in Client Device set
set_sub_factor // unqiue sub_factor assigned to a client device
// 0 <= set_sub_factor < set_device_count 

//Message Index must satisfy
assert( ( message_index % set_device_count ) === set_sub_factor )
```

###### `client_device1`'s message table

`set_sub_factor = 0`

| message_index | reciver_id | sender_id | data |
|--|--|--|--|
| 3 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...100>` |
| 6 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...430>` |

###### `client_device2`'s message table

`set_sub_factor = 1`

| message_index | reciver_id | sender_id | data |
|--|--|--|--|
| 1 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...400>` |
| 4 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...230>` |

###### `client_device3`'s message table

`set_sub_factor = 2`

| message_index | reciver_id | sender_id | data |
|--|--|--|--|
| 2 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...430>` |
| 5 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...550>` |

##### *If* `client_device3` goes offline for more than a `offline_threshold` limit before reciving the above messages

In this scenario, the load of `client_device3` is moved, either to `client_device2` or `client_device3` based on their availability or any other suitable strategy.

So lets assume our asserted stretegy diverts the load of `client_device3` to `client_device2`. Then the table of recived messages would look like.

```js
//Client Device set represented on client document
client = {
   client_id: "abcd88u21noinoin3332",
   devices: [
      { device_id: "sbcd8noinoin33328u21", state_conncted: true, set_sub_factors: [0] },
      { device_id: "sbcd8noinoin33328u22", state_conncted: true, set_sub_factors: [1,2] },
      { device_id: "sbcd8noinoin33324u00", state_conncted: false, set_sub_factors: [] },
   ]
}
```

###### `client_device1`'s message table

`set_sub_factor = 0`

| message_index | reciver_id | sender_id | data |
|--|--|--|--|
| 3 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...100>` |
| 6 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...430>` |

###### `client_device2`'s message table

`set_sub_factor = 1, 2`

| message_index | reciver_id | sender_id | data |
|--|--|--|--|
| 1 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...400>` |
| 2 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...430>` |
| 4 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...230>` |
| 5 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...550>` |




