

### index.js

``` javascript
var app = require('express')();
var server = require('http').createServer(app);
var io = require('socket.io')(server);

app.set('view engine', 'ejs'); 
app.set('views',  __dirname + '/views');  

app.get('/', (req, res) => {
    res.render('index');
})

io.on('connection', (socket) => {   
    socket.emit('usercount', io.engine.clientsCount);

    socket.on('message', (name, msg) => {
        // console.log(name+ ': ' + msg);

        io.emit('message', name, msg);
    });
});

server.listen(3000, function() {
  console.log('Listening on http://localhost:3000/');
});
```



### index.ejs

``` ejs
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Socket.IO</title>
</head>
<body>
    <button id="btn">닉네임 추가</button>
    <p id="namep">익명</p>
    <ul id="messages" type="none">
        <li id="usercount"></li>
    </ul>

    <form id="msgform">
        <input id="msginput" autocomplete="off" type="text">
        <button type="submit">전송</button>
    </form>

    <script src="/socket.io/socket.io.js"></script>
    <script>
        const socket = io();
        const msgform = document.getElementById('msgform');
        const namep = document.getElementById('namep');

        window.onload = function () {
        const btn = document.getElementById("btn");
        btn.onclick = name;
        }
        function name() {
            const name = prompt('닉네임 입력하세요');
            if(name !== null) {
                namep.value = name;
                namep.innerHTML = name;
            }
        }
 
        socket.on('usercount', (count) => {
            const userCounter = document.getElementById('usercount');
            userCounter.innerText = "현재 " + count + "명이 서버에 접속해있습니다.";
        });

        socket.on('message', (name, msg) => {
            const messageList = document.getElementById('messages');
            const messageTag = document.createElement("p");
            if(name === null) name="익명";
            messageTag.innerText = name+": "+msg;
            messageList.appendChild(messageTag);
        });

        msgform.onsubmit = (e) => {
            e.preventDefault();
            const msginput = document.getElementById('msginput');
            const namep = document.getElementById('namep');

            socket.emit('message', namep.value, msginput.value);

            msginput.value = "";
        };
    </script>
</body>
</html>
```



### html 대신 ejs를 사용한 이유

-  클라이언트 요청에 따라 웹문서 들어가는 내용(결과)이 달라질 수 있어서 정적인 부분과 동적인 부분을 따로 하기위해 사용