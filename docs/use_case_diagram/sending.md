## Sending message

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
