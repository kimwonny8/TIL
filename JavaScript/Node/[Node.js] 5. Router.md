routes 폴더 생성 후 `index.js`, `user.js` 

1단계 url 분리!!



### index.js

```javascript
const express = require('express');
const path = require('path');

const router = express.Router();

// 보낼때 /users 보냈고 그 뒤 주소만 보는거 즉, users/ 이후 주소만 볼 수 있음
router.get('/', (req, res, next) =>{
    res.sendFile(path.join(__dirname, '/index.html'));
})

module.exports = router;
```



### users.js

``` javascript
const express = require('express');

const router = express.Router();

router.get('/', (req, res, next) => {
    const users = ['a','b','c']; // 디비 없으니까 임의로 생성
    res.json(users); // send나 json
});
module.exports = router;
```



### app.js

- 위치 잘 지킬것!!! 

``` javascript
const express = require('express');
const path = require('path');
const dotenv = require('dotenv');
const morgan = require('morgan');
const cookieParser = require('cookie-parser');

const indexRouter = require('./routes'); // index.js는 생략 가능
const usersRouter = require('./routes/users');

dotenv.config();

const app = express();
app.set('port', process.env.PORT || 3000);

app.use(morgan('dev'));
app.use('/', express.static(path.join(__dirname, 'static'))); 
app.use(express.json()); 
app.use(express.urlencoded({ extended: false })); 
app.use(cookieParser(process.env.COOKIE_SECRET)); 
 

// 주소가 들어오면 저 라우터로 보내주세욤
app.use('/', indexRouter);
app.use('/users', usersRouter);


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



## 라우트 매개변수

주소에 있는걸 쓰는 게 파라미터 `/:param`

- 일반 라우터보다 뒤에 위치해야 함
- 아니면 파라미터로 인식하기 때문

``` javascript
router.get('/:id', (req, res, next) => {
    const id = req.params.id;
    res.send(id);
})
```



### 쿼리스트링

```javascript
router.get('/', (req, res, next) => {
    const users = ['a','b','c']; 
     
    // 쿼리스트링 /users?name=test&age=20
    const name = req.query.name || 'unknown';
    const age = req.query.age || 20;
    console.log(name, age);
    res.json(users); // send나 json
});
```

- 프라이머리키 아니고, 검색하는 경우 주로 사용 `/menu?type=coffee`

- /users GET: 모든 사용자 조회
- /users POST : 사용자 추가
- /users/:id GET:해당아이디를 가진 사용자 조회
- /users/:id PUT/PATCH : 사용자 정보 수정
- /users/:id DELETE : 사용자 삭제



## req, res

## req

- `req.app`: req 객체를 통해 app 객체에 접근할 수 있습니다. `req.app.get('port')`
- `req.body`: body-parser 미들웨어가 만드는 요청의 본문을 해석한 객체입니다. 
- `req.cookies`: cookie-parser 미들웨어가 만드는 요청의 쿠키를 해석한 객체입니다. 
- `req.ip`: 요청의 ip 주소가 담겨 있습니다. 
- `req.params`: 라우트 매개변수에 대한 정보가 담긴 객체입니다. 
- `req.query`: 쿼리스트링에 대한 정보가 담긴 객체입니다.
- `req.signedCookies`: 서명된 쿠키들은 req.cookies 대신 여기에 담겨 있습니다. 
- `req.get(헤더 이름)`: 헤더의 값을 가져오고 싶을 때 사용하는 메서드입니다.



## res

- `res.app`: req.app처럼 res 객체를 통해 app 객체에 접근할 수 있습니다. 
- `res.cookie(키, 값, 옵션)`: 쿠키를 설정하는 메서드입니다.
- `res.clearCookie(키, 값, 옵션)`: 쿠키를 제거하는 메서드입니다. 
- `res.setHeader(헤더, 값)`: 응답의 헤더를 설정합니다. 
- `res.status(코드)`: 응답 시의 HTTP 상태 코드를 지정합니다. 404, 500 등



**응답은 한 번만 보내야 함** (리턴같은 애라서 리턴은 한번만!!) ⭐

- `res.end()`: 데이터 없이 응답을 보냅니다. 
- `res.json(JSON)`: JSON 형식의 응답을 보냅니다. 
- `res.redirect(주소)`: 리다이렉트할 주소와 함께 응답을 보냅니다. 
- `res.render(뷰, 데이터)`: 다음 절에서 다룰 템플릿 엔진을 렌더링해서 응답할 때 사용하 는 메서드입니다. 
- `res.send(데이터)`: 데이터와 함께 응답을 보냅니다. 데이터는 문자열일 수도 있고H  TML일 수도 있으며, 버퍼일 수도 있고 객체나 배열일 수도 있습니다. 
- `res.sendFile(경로)`: 경로에 위치한 파일을 응답합니다. 




- 메서드 체이닝 지원

``` javascript
res.status(403).json({result:'err'});
```

