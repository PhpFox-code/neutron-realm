Real time multiple layer support library for phpfox
============================================

Prototype
```php
1. $message =  new Message();
2. $recipinents  =  (select friend of sender).result;
3. $pusher->push($message, $recipinents);
```

1. Create new message to push.

```php
$message  = new Message([
    id: int, identity of message,
    name: string, type of message,
    sender_id: int, who send the message,
    data: array, 
    link: [], where to redirected,
    title: string, title of mesage,
    summary: string, some line describe about message,
    creation: datetime, when was this posted,
    expiration: int , when it will expires,
]);
```

2. Working with socket layer
- Each users may connect to multiple servers (scaling).
- Each message must be relayed on socket servers.
- For maintenance
  * We support only NodeJS socket server.
  * Provide cloud NodJS server for the client who have no NodeJs setup environment or testing.

Solution 01:
- Post Message:
 * Use normal Ajax post to web server.
 * Response to the client that message is posted.
 * Server will relay on socket server to deliver messages.

- Get Message Via WebSocket  
  * Keep clients connections.
  * Socket server handle request as a proxy and connection container.
  * Socket serverIn async way, no waiting and block IO. It get message, keep it in message container, then send to message delivery.
  
Solution 02
- Post Message:
 * Use normal Ajax post to web server.
 * Response to the client that message is posted.
 * Server will post to a message queue service (Local, SQS, ...)
  
- Get Message Via WebSocket
 * Keep clients connections, connect to message queue as sub-pub prototype.

3. Long Polling Request:
 
- Post Message:
 * Use normal Ajax post to web server.
 * Response to the client that message is posted.
 * Do nothing
 
- Get Message
 * Clients send ajax request to server.
 * Server is looking up the messages
 * If there are no new message, it will be idle then re-try after [x] times.
 *

5. Using custom push service [working as web socket].
 * [pusher] (https://pusher.com/)
 * ...
 
 