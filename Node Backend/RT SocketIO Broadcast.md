# Can you demonstrate how you would create a real-time chat API endpoint using Node.js and the Socket.io library, and use it to broadcast messages to all connected clients?
  
```typescript
const app = require('express')();
const http = require('http').createServer(app);
const io = require('socket.io')(http);

app.get('/', (req, res) => {
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', (socket) => {
  console.log('a user connected');

  socket.on('chat message', (msg) => {
    console.log('message: ' + msg);
    io.emit('chat message', msg);
  });

  socket.on('disconnect', () => {
    console.log('user disconnected');
  });
});

http.listen(3000, () => {
  console.log('listening on *:3000');
});

```

In this implementation, we first create an Express app and an HTTP server using the **`createServer`** method. We then create a Socket.io instance that listens on the server using the **`io`** function.

We define a route for the root path (**`'/'`**) that serves an HTML file that contains the chat client UI.

We use the **`io.on('connection', ...)`** method to handle new client connections. When a client connects, we log a message to the console and register two event listeners on the **`socket`** object: one for the **`'chat message'`** event, and one for the **`'disconnect'`** event.

The **`'chat message'`** event listener receives a message string from the client, logs it to the console, and broadcasts it to all connected clients using the **`io.emit('chat message', msg)`** method.

The **`'disconnect'`** event listener logs a message to the console when a client disconnects.

Finally, we listen on port 3000 and log a message to the console when the server starts.

To use this chat API endpoint, clients can connect to the server using Socket.io on the front-end and send messages using the **`'chat message'`** event. The server will broadcast the message to all connected clients using the **`'chat message'`** event as well.