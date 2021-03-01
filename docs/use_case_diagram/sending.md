## Sending message

### In device

**Local message queuing table**

| queue_index | message_index | sender_id | reciver_id | data |
|--|--|--|--|--|
| 1 | NULL | abcd88u21noinoin3332 | abcd88u21noinoin8787 | `<Buffer..."Hi"...101>` |
| 2 | NULL | abcd88u21noinoin3332 | abcd88u21noinoin8787 | `<Buffer..."Bye"...102>` |

When message is added to MSS

| queue_index | message_index | sender_id | reciver_id | data |
|--|--|--|--|--|
| 1 | 7 | abcd88u21noinoin3332 | abcd88u21noinoin8787 | `<Buffer..."Hi"...101>` |
| 2 | 11 | abcd88u21noinoin3332 | abcd88u21noinoin8787 | `<Buffer..."Bye"...102>` |

### In Online MSS

***Client's document on MongoDB collection***
```js
client = {
   client_id: abcd88u21noinoin3332,
   recive_index: 0
}
```

***Client's data snipet from table of Masterss Keysapce***
| message_index | reciver_id | sender_id | data |
|--|--|--|--|

When ever a new message is added We perform

```js
//DOCUMENT DB
Update(
  { client_id: abcd88u21noinoin3332 },
  { $inc: {recive_index:1} }            //+1 increment on recive_index
)
```

```js
//Master Keyspace
Insert(
  {
    message_index: 1,
    reciver_id: abcd88u21noinoin3332,
    sender_id: abcd88u21noinoin8787,
    data: <Buffer...400>
  }
)
```

**Resulting**

MongoDB collection


```js
client = {
   client_id: abcd88u21noinoin3332,
   recive_index: 1
}
```


Masterless Keyspace Table

| message_index | reciver_id | sender_id | data |
|--|--|--|--|
| 1 | abcd88u21noinoin3332  | abcd88u21noinoin8787 | `<Buffer...400>` |

