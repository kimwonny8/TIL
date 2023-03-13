node.js가 설치되어있다는 걸 가정하고, 시작점은 `index.js` 로 두고 진행한다.



## 👉 express 통신 확인

모듈 설치

```
npm i express@4
```



### index.js

```javascript
const express = require('express');
const app = express();
const http = require('http');
const server = http.createServer(app);

app.get('/', (req, res) => {
  res.send('<h1>Hello world</h1>');
});

server.listen(3000, () => {
  console.log('listening on *:3000');
});
```

- Express는 `app`HTTP 서버에 제공할 수 있는 함수 핸들러로 초기화됩니다(4행에서 볼 수 있음).
- `/`웹 사이트 홈에 도달했을 때 호출되는 경로 핸들러를 정의합니다 .
- http 서버가 포트 3000에서 수신 대기하도록 합니다.

이 후 `localhost:3000` 열면 `Hello world`가 떠있는걸 확인할 수 있다.



## 👉 socket.io

모듈 설치

```
npm i socket.io
```



### index.js

```javascript
const express = require('express');
const app = express();
const http = require('http');
const server = http.createServer(app);
const { Server } = require("socket.io");
const io = new Server(server);

app.get('/', (req, res) => {
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', (socket) => {
  console.log('a user connected');
});

server.listen(3000, () => {
  console.log('listening on *:3000');
});
```



### index.html

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Socket.IO chat</title>
    <style>
      body { margin: 0; padding-bottom: 3rem; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; }

      #form { background: rgba(0, 0, 0, 0.15); padding: 0.25rem; position: fixed; bottom: 0; left: 0; right: 0; display: flex; height: 3rem; box-sizing: border-box; backdrop-filter: blur(10px); }
      #input { border: none; padding: 0 1rem; flex-grow: 1; border-radius: 2rem; margin: 0.25rem; }
      #input:focus { outline: none; }
      #form > button { background: #333; border: none; padding: 0 1rem; margin: 0.25rem; border-radius: 3px; outline: none; color: #fff; }

      #messages { list-style-type: none; margin: 0; padding: 0; }
      #messages > li { padding: 0.5rem 1rem; }
      #messages > li:nth-child(odd) { background: #efefef; }
    </style>
  </head>
  <body>
    <ul id="messages"></ul>
    <form id="form" action="">
      <input id="input" autocomplete="off" /><button>Send</button>
    </form>
  </body>
</html>
```

- 채팅방 모양을 대충 구현해준다.
- 이 후 아래의 코드를 추가한다!

```html
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io();

  var messages = document.getElementById('messages');
  var form = document.getElementById('form');
  var input = document.getElementById('input');

  form.addEventListener('submit', function(e) {
    e.preventDefault();
    if (input.value) {
      socket.emit('chat message', input.value);
      input.value = '';
    }
  });

  socket.on('chat message', function(msg) {
    var item = document.createElement('li');
    item.textContent = msg;
    messages.appendChild(item);
    window.scrollTo(0, document.body.scrollHeight);
  });
</script>
```



- `io.emit()` : 모든 사람에게 이벤트를 보내기 위한 메소드

``` javascript
io.emit('some event', { someProperty: 'some value', otherProperty: 'other value' }); 
```

``` javascript
io.on('connection', (socket) => {
  socket.on('chat message', (msg) => {
    io.emit('chat message', msg);
  });
});
```



- `broadcast` : 특정 방출 소켓을 제외한 모든 사람에게 메시지를 보냄

``` javascript
io.on('connection', (socket) => {
  socket.broadcast.emit('hi');
});
```





references : [공식 문서](https://socket.io/get-started/chat)