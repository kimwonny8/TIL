|           Node.js           |                                                              |
| :-------------------------: | :----------------------------------------------------------: |
|            http             |                 웹서버 만들어주는 라이브러리                 |
| Express, Koa, Hapi, Nest 등 | (http 위에 올려놓고 쓰는 플랫폼) => express 서버 짤줄압니다! |





## 📌 Express

node를 설치하면 http 라이브러리가 자동으로 설치되는데, 코드가 보기 좋지 않고, 확장성도 떨어진다. 

그걸 간편하게 만든게 Express!



### text 보내기

``` javascript
// app.js

const express = require('express');

const app = express();
app.set('port', process.env.PORT || 3000); // set - 전역변수로 설정(변수이름, 값)

app.get('/', (req, res) => {
    res.send('Hello, Express'); // send - text만 보냄
});

app.listen(app.get('port'), () => {
    console.log(app.get('port'), '빈 포트에서 대기 중'); // port 서버 잘 켜지면 콜백 실행
});
```

- `app.set(‘port’, 포트)` : 서버가 실행될 포트 지정 
- `app.get(‘주소’, 라우터) ` : GET 요청이 올 때 어떤 동작을 할지 지정
- `app.listen(‘포트’, 콜백)` : 몇 번 포트에서 서버를 실행할지 지정





### html 파일로 보여주기

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <h1>익스프레스</h1>
</body>
</html>
```

```javascript
// app.js
const express = require('express');
const path = require('path');

const app = express();
app.set('port', process.env.PORT || 3000);

app.get('/', (req, res) => {
    res.sendFile(path.join(__dirname, '/index.html'));
});

app.listen(app.get('port'), () => {
    console.log(app.get('port'), '포트에서 대기 중');
});
```





## 📌 미들웨어

- express는 미들웨어로 구성된다.

- 요청과 응답의 중간에 위치하고, `app.use(미들웨어)`로 장착
- 위에서 아래로 순서대로 실행됨
- req, res, next가 매개변수인 함수 
  - req: 요청, res: 응답 조작 가능 
- next()로 다음 미들웨어로 넘어간다.

``` javascript
app.use((req, res, next) => {
    console.log('모든 요청이 거쳐갑니다.');
    next();
})
```



## 👉 자주 쓰는 미들웨어

- morgan, cookie-parser, express-session



### env

`.env` 파일 생성 후 `PORT=값`이나  `COOKIE_PARSER=값` 설정

``` javascript
const express = require('express');
const dotenv = require('dotenv');

dotenv.config();
const app = express();
app.set('port', process.env.PORT || 3000);
```



### morgan

- 서버로 들어온 요청과 응답을 기록해주는 미들웨어(로그 기록)



### static

- 정적인 파일들을 제공하는 미들웨어(모든 요청이 controller로 넘어가지X)
- get 까지 가기 전에 처리

``` javascript
app.use('/', express.static(path.join(__dirname, 'static'))); 
// localhost:4000/test.jpg 라고 들어가면 static 폴더에서 test파일 찾아서 보여줌
```



### body-parser ⭐

- 요청의 본문을 해석해주는 미들웨어
- request 로 들어오는 각종 데이터 처리



### cookie-parser

- 요청 헤더의 쿠키를 해석해주는 미들웨어
- `.env` 파일에 `COOKIE_PARSER=값` 추가해서 사용



### express-session

- 세션 관리용 미들웨어



### 미들웨어 예시코드

**순서 중요함**

```javascript
const express = require('express');
const path = require('path');
const dotenv = require('dotenv');
const morgan = require('morgan');
const cookieParser = require('cookie-parser');

dotenv.config();

const app = express();
app.set('port', process.env.PORT || 3000);

app.use(morgan('dev'));
app.use('/', express.static(path.join(__dirname, 'static'))); 
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser(process.env.COOKIE_SECRET));


app.use((req, res, next) => {
    console.log('모든 요청이 거쳐갑니다.');
    next();
})
```



### express.json()

- json String

```json
{
	"id":123,
	"name": "abc"
}
```

- 자바의 객체로 파싱하면

```json
{
	id:123,
	name: "abc"
}
```



``` javascript
const express = require('express');
const path = require('path');
const dotenv = require('dotenv');
const morgan = require('morgan');
const cookieParser = require('cookie-parser');

dotenv.config();

const app = express();
app.set('port', process.env.PORT || 3000);

app.use(morgan('dev'));
app.use('/', express.static(path.join(__dirname, 'static'))); 
// localhost:4000/test.jpg 라고 들어가면 static 폴더에서 test파일 찾아서 보여주고 끝
// 못찾으면 아래로 넘어감
app.use(express.json()); // json String -> 객체로
app.use(express.urlencoded({ extended: false })); // 쿼리스트링 인코딩
app.use(cookieParser(process.env.COOKIE_SECRET)); 
 

// 주소 명시 x, 모든 요청이 거쳐감
app.use((req, res, next) => {
    console.log('모든 요청이 거쳐갑니다.');
    next(); // 아래로로
})

// 해당 주소만 처리
app.get('/', (req, res) => {
    // res.send('Hello, Express');
    res.sendFile(path.join(__dirname, '/index.html'));
});

// 위 주소가 아닐 때 404 띄울 use
app.use((req, res, next) => {
    const err = new Error('wrong request');
    err.status = 404;
    next(err);
});

// 채워주면 그 값 쓰고 아니면 500 써라 -> 공용 모듈
app.use((err, req, res) => {
    const status = req.status || 500;
    // error 출력하는 ui render
});


app.listen(app.get('port'), () => {
    console.log(app.get('port'), '포트에서 대기 중');
});
```

- router 분리할것
