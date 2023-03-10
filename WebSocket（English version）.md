WebSocket is a TCP-based Network Communication Protocol that provides a way to maintain a long connection between a client and a server, allowing two-way communication and realizing real-time communication.

## 1 Detailed description of the working principle, application scenarios and advantages of WebSocket:

1.1 Working principle

WebSocket works based on the Upgrade header of the HTTP/1.1 protocol, which enables two-way communication by establishing a persistent connection between the client and the server, allowing the server to push data to the client, and allowing the client to send data to the server.  The client requests to be upgraded to the WebSocket protocol by including the Upgrade field in the HTTP request header. When the server responds to the request, it also includes the Upgrade field, indicating that the WebSocket protocol is supported. Then a WebSocket connection is established between the Client and the server, and both parties can send and receive data through this connection.

1.2 Common application scenarios

1.2.1 Live chat: WebSocket enables real-time chat, such as online customer service, social applications, real-time collaboration tools, and more.  1.2.2 Games: WebSocket can realize real-time games, such as online chess and cards, multiplayer games, etc.  1.2.3 Stock Quotes: WebSocket can get stock quotes in real time, such as financial applications, securities trading, etc.  1.2.4 Real-time monitoring: WebSocket can achieve real-time monitoring, such as network monitoring, Internet of Things, etc.  1.2.5 Others: WebSocket can also be used in some other application scenarios, such as online editors, file transfers, etc.

1.3 Advantages

1.3.1 Real-time: WebSocket provides real-time communication, and a persistent connection is established between the Client and the server, which can realize real-time data transmission.  1.3.2 Two-way communication: WebSocket allows two-way communication, the server can send data to the client, the client can also send data to the server.  1.3.3 Performance optimization: WebSockets can reduce network traffic, reduce server load, and improve application performance.  1.3.4 Compatibility: WebSocket has good compatibility, supports a variety of browsers and operating systems, and can run on Web, iOS, Android and other platforms.

## 2 Code use case for WebSocket

### 2.1 Connect to the server and send/receive messages using the WebSocket API

```JavaScript
// ?????? WebSocket ??????
const socket = new WebSocket('wss://example.com');

// ??????????????????????????????
socket.onopen = () => {
  console.log('WebSocket ???????????????');

  // ????????????????????????
  socket.send('Hello server!');
};

// ????????????????????????????????????
socket.onmessage = (event) => {
  console.log('??????????????????', event.data);

  // ?????????????????? JSON ??????
  const data = JSON.parse(event.data);

  // ????????????
  if (data.type === 'chat') {
    console.log(data.username + ': ' + data.message);
  } else {
    console.log('?????????????????????', data.type);
  }
};

// ?????? WebSocket ??????
socket.onerror = (error) => {
  console.error('WebSocket ?????????', error);
};

// ?????? WebSocket ??????
socket.onclose = (event) => {
  console.log('WebSocket ??????????????????', event.code, event.reason);
};

// ???????????? JSON ??????
const message = {
  type: 'chat',
  username: 'Alice',
  message: 'Hello, everyone!'
};

socket.send(JSON.stringify(message));

//????????????????????? WebSocket ???????????????????????????????????????????????????????????????????????????
//?????????????????? JSON.parse() ??? JSON.stringify() ?????????????????? JSON ???????????????
```

### 2.2 Using WebSocket for Binary Data Processing and Heartbeat Detection

```JavaScript
// ?????? WebSocket ??????
const socket = new WebSocket('wss://example.com');

// ????????????????????????
const sendHeartbeat = () => {
  const heartbeat = new ArrayBuffer(2);
  const view = new Uint16Array(heartbeat);
  view[0] = 0x1234;
  socket.send(heartbeat);
};

// ?????? WebSocket ??????????????????
socket.onopen = () => {
  console.log('WebSocket ???????????????');

  // ??????????????????
  setInterval(sendHeartbeat, 30000);
};

// ?????? WebSocket ???????????????
socket.binaryType = 'arraybuffer';
socket.onmessage = (event) => {
  console.log('???????????????????????????', event.data);

  // ?????????????????????
  const buffer = event.data;
  const view = new Uint16Array(buffer);
  console.log('?????????????????????', view[0]);
};

// ?????? WebSocket ??????
socket.onerror = (error) => {
  console.error('WebSocket ?????????', error);
};

// ?????? WebSocket ??????
socket.onclose = (event) => {
  console.log('WebSocket ??????????????????', event.code, event.reason);
};

// ???????????????????????????
const buffer = new ArrayBuffer(4);
const view = new Uint16Array(buffer);
view[0] = 0x1234;
view[1] = 0x5678;
socket.send(buffer);

//??????????????? setInterval() ????????????????????????????????????????????? WebSocket ????????????????????????
//??????????????? ArrayBuffer ??? Uint16Array ????????????????????????
//???????????? WebSocket ??????????????? socket.binaryType ???????????? 'arraybuffer'?????????????????????????????????????????????
```

## 3 Business scenario use cases for WebSocket

### 3.1 Setting up a chat room

```JavaScript
//???????????????????????? Node.js??????
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8080 });

// ??????????????????????????????
const clients = new Set();

// ????????????????????????
wss.on('connection', (ws) => {
  console.log('?????????????????????');

  // ????????????????????????
  clients.add(ws);

  // ?????????????????????
  ws.on('message', (message) => {
    console.log('??????????????????', message);

    // ??????????????????????????????
    for (let client of clients) {
      if (client.readyState === WebSocket.OPEN) {
        client.send(message);
      }
    }
  });

  // ???????????????????????????
  ws.on('close', () => {
    console.log('????????????????????????');

    // ???????????????????????????
    clients.delete(ws);
  });
});

//???????????????
<!DOCTYPE html>
<html>
<head>
  <title>WebSocket ?????????</title>
</head>
<body>
  <input type="text" id="message" placeholder="????????????">
  <button id="send">??????</button>
  <ul id="chat"></ul>

  <script>
    // ?????? WebSocket ??????
    const ws = new WebSocket('ws://localhost:8080');

    // ?????? WebSocket ??????
    ws.onmessage = (event) => {
      const message = event.data;
      console.log('??????????????????', message);

      // ???????????????????????????
      const chat = document.getElementById('chat');
      const li = document.createElement('li');
      li.textContent = message;
      chat.appendChild(li);
    };

    // ????????????
    const sendButton = document.getElementById('send');
    sendButton.addEventListener('click', () => {
      const message = document.getElementById('message').value;
      console.log('???????????????', message);

      ws.send(message);
    });
  </script>
</body>
</html>

//??????????????????????????? 8080 ???????????????????????????????????????????????????????????????????????????????????????
//??????????????? JavaScript ?????? WebSocket ?????????????????????????????????????????????????????????????????????????????????????????????????????????
```

### 3.2 Build real-time collaboration tools

```JavaScript
//???????????????????????? Node.js ??? Express ????????????
const express = require('express');
const http = require('http');
const WebSocket = require('ws');

const app = express();
const server = http.createServer(app);
const wss = new WebSocket.Server({ server });

// ??????????????????????????????
const clients = new Set();

// ????????????????????????
wss.on('connection', (ws) => {
  console.log('?????????????????????');

  // ????????????????????????
  clients.add(ws);

  // ?????????????????????
  ws.on('message', (message) => {
    console.log('??????????????????', message);

    // ??????????????????????????????
    for (let client of clients) {
      if (client.readyState === WebSocket.OPEN) {
        client.send(message);
      }
    }
  });

  // ???????????????????????????
  ws.on('close', () => {
    console.log('????????????????????????');

    // ???????????????????????????
    clients.delete(ws);
  });
});

// ???????????????
server.listen(8080, () => {
  console.log('??????????????????');
});

//???????????????
<!DOCTYPE html>
<html>
<head>
  <title>??????????????????</title>
</head>
<body>
  <textarea id="editor"></textarea>

  <script>
    // ?????? WebSocket ??????
    const ws = new WebSocket('ws://localhost:8080');

    // ?????? WebSocket ??????
    ws.onmessage = (event) => {
      const message = event.data;
      console.log('??????????????????', message);

      // ?????????????????????
      const editor = document.getElementById('editor');
      editor.value = message;
    };

    // ?????????????????????
    const editor = document.getElementById('editor');
    editor.addEventListener('input', () => {
      const content = editor.value;
      console.log('???????????????', content);

      ws.send(content);
    });
  </script>
</body>
</html>

//??????????????? 8080 ???????????????????????????????????????????????????????????????????????????????????????
//??????????????? JavaScript ?????? WebSocket ???????????????????????????????????????????????????????????????????????????????????????????????????
```

## 4 WebSocket Security

### 4.1 Using SSL/TLS encryption

  The SSL/TLS protocol can encrypt the WebSocket connection to protect the confidentiality and integrity of the data. Usually, the communication between the WebSocket and the server is unencrypted, which makes the data vulnerable to eavesdropping and tampering. The SSL/TLS protocol can encrypt the communication data and protect the security of the WebSocket connection.

```JavaScript
//?????? Node.js ????????? WebSocket SSL/TLS ??????????????????,
//????????????????????????????????? SSL/TLS ???????????????????????? WebSocket ????????????
const https = require('https');
const WebSocket = require('ws');
const fs = require('fs');

const options = {
    key: fs.readFileSync('/path/to/ssl/key.pem'),
    cert: fs.readFileSync('/path/to/ssl/cert.pem')
};

const server = https.createServer(options);
const wss = new WebSocket.Server({ server });

wss.on('connection', (ws) => {
    console.log('WebSocket connection established');

    ws.on('message', (message) => {
        console.log(`Received message: ${message}`);
    });

    ws.send('WebSocket connection established');
});

server.listen(8080, () => {
    console.log(`Server started on port ${server.address().port}`);
});
//?????? Node.js ??? https ????????????????????? SSL/TLS ??????????????????????????? WebSocket ????????????
//key ??? cert ??????????????? SSL/TLS ???????????????
//???????????????????????? WebSocket ????????????????????? connection ??????????????????????????? WebSocket ??????
//??? WebSocket ??????????????????????????????????????????????????????????????????????????????????????????

//?????????
//???????????? SSL/TLS ??????????????????????????? WebSocket ??? WebSocket() ??????????????????????????? WebSocket ??????
const ws = new WebSocket('wss://example.com', {
    ca: [fs.readFileSync('/path/to/ssl/cert.pem')],
    rejectUnauthorized: true
});

ws.on('open', () => {
    console.log('WebSocket connection established');
});

ws.on('message', (message) => {
    console.log(`Received message: ${message}`);
});

ws.send('Hello, WebSocket!');

//??????????????? wss:// ????????????????????? SSL/TLS ????????? WebSocket ???????????????????????? SSL/TLS ???????????????
//?????????????????? rejectUnauthorized ????????? true?????????????????????????????????????????????????????????
```

###   4.2 Verifying WebSocket Handshake Requests

  A WebSocket handshake request is an HTTP request that an attacker can forge a handshake request in an attempt to establish a malicious WebSocket connection. To prevent this attack, the WebSocket handshake request can be verified to verify the legitimacy of the request. For example, the fields in the request header can be checked to prevent attackers from forging the request header.

```JavaScript
//????????????????????????????????????????????? WebSocket ??????????????????????????????
const WebSocket = require('ws');
const crypto = require('crypto');

const server = new WebSocket.Server({ port: 8080 });

server.on('connection', (ws, req) => {
    const key = req.headers['sec-websocket-key'];
    const accept = generateAccept(key);

    if (!validateRequest(req)) {
        console.log('Invalid WebSocket request');
        ws.close(4001, 'Invalid WebSocket request');
        return;
    }

    console.log('WebSocket connection established');

    ws.on('message', (message) => {
        console.log(`Received message: ${message}`);
    });

    ws.send('WebSocket connection established');
});

function validateRequest(req) {
    const headers = req.headers;

    if (headers['sec-websocket-version'] !== '13') {
        return false;
    }

    if (!headers['sec-websocket-key']) {
        return false;
    }

    return true;
}

function generateAccept(key) {
    const GUID = '258EAFA5-E914-47DA-95CA-C5AB0DC85B11';
    const sha1 = crypto.createHash('sha1');
    sha1.update(key + GUID);
    const accept = sha1.digest('base64');
    return accept;
}

//?????? validateRequest() ??????????????? WebSocket ????????????
//????????????????????? sec-websocket-version ????????????????????? 13??????????????? sec-websocket-key ????????????????????????
//??????????????????????????????????????????????????????????????? false??????????????? WebSocket ??????
//??? generateAccept() ??????????????????????????? sec-websocket-accept ????????????????????????????????? WebSocket ????????????
//???????????????????????? WebSocket ?????????????????????????????? req.headers ?????????????????????????????????????????????
//?????????req.headers['sec-websocket-key'] ???????????? sec-websocket-key ??????????????????
//?????????????????????????????????????????? sec-websocket-version ??? sec-websocket-key ????????????????????????????????????
```

###   4.3 Restricting WebSocket Connections

  To prevent malicious attacks, you can limit WebSocket connections. For example, you can limit the connection rate, set the maximum number of connections, limit the connection source, etc. These measures can reduce the attack effect of attackers and protect the security of WebSocket connections.

```JavaScript
//???????????? WebSocket ???????????????
const WebSocket = require('ws');

const server = new WebSocket.Server({ port: 8080 });

// ????????????????????????
server.on('connection', (socket) => {
  console.log('Client connected');

  // ???????????????????????? 10
  if (server.clients.size > 10) {
    // ?????????????????????????????????
    socket.send('Sorry, the server is full');
    socket.close();
  }

  // ????????????????????????
  socket.on('close', () => {
    console.log('Client disconnected');
  });
});

//????????????????????? WebSocket ???????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????
```

###   4.4 Preventing Cross-Site Scripting Attacks

  Cross-site scripting (XSS) is a common network attack in which attackers can inject malicious scripts into web applications, steal user data or perform malicious actions. To prevent XSS attacks, input data can be filtered and verified to prevent malicious scripts from injecting WebSocket connections.

```JavaScript
//?????????????????????????????????????????? WebSocket ?????????????????????
const WebSocket = require('ws');
const crypto = require('crypto');

const server = new WebSocket.Server({ port: 8080 });

// ?????????????????????
const secretKey = crypto.randomBytes(16).toString('hex');

// ????????????????????????
server.on('connection', (socket, request) => {
  console.log('Client connected');

  // ????????????????????????
  const clientKey = request.headers['sec-websocket-key'];

  // ?????????????????????????????????????????????????????????
  const hash = crypto.createHash('sha1')
    .update(clientKey + secretKey)
    .digest('base64');

  // ???????????????
  socket.write('HTTP/1.1 101 Switching Protocols\r\n' +
               'Upgrade: websocket\r\n' +
               'Connection: Upgrade\r\n' +
               `Sec-WebSocket-Accept: ${hash}\r\n\r\n`);

  // ????????????????????????
  socket.on('close', () => {
    console.log('Client disconnected');
  });
});

//????????????????????? WebSocket ????????????????????????????????????????????????????????????????????????????????????????????????????????????
//??????????????????????????????????????????????????????????????????????????????????????? WebSocket ?????????????????????????????????????????????
```

###   4.5 Use of Authentication and Authorization

  Authentication and authorization is an important way to protect WebSocket connections. It can verify user identity and authorize user access. For example, authentication and authorization protocols such as OAuth2.0 can be used to secure WebSocket connections.

```JavaScript
//???????????????????????????????????? WebSocket ?????????????????????
const WebSocket = require('ws');
const jwt = require('jsonwebtoken');

const server = new WebSocket.Server({ port: 8080 });

// ??????????????????
const secretKey = 'mysecretkey';

// ????????????????????????
server.on('connection', (socket, request) => {
  console.log('Client connected');

  // ?????????????????????????????????
  const authHeader = request.headers.authorization;
  const token = authHeader &amp;&amp; authHeader.split(' ')[1];

  if (!token) {
    // ????????????????????????????????????????????????????????????
    socket.send('Unauthorized');
    socket.close();
    return;
  }

  try {
    // ??????????????????
    const decoded = jwt.verify(token, secretKey);

    // ????????????????????????????????? ID
    socket.userId = decoded.userId;
  } catch (error) {
    // ??????????????????????????????????????????????????????
    socket.send('Invalid token');
    socket.close();
    return;
  }

  // ????????????????????????
  socket.on('close', () => {
    console.log('Client disconnected');
  });
});

//????????????????????? WebSocket ????????????????????? JSON Web Tokens (JWT) ??????????????????????????????
//????????????????????????????????????????????????????????????????????????????????????????????????
//???????????????????????????????????? ID ?????????????????????????????????????????????????????????
//????????????????????????????????????????????????????????????
```