## 📌 Pug

- depth 가 깊은 html 에서 부모 자식 바꾸고 이런 작업 간편함

```
npm i pug
```

```javascript
// app.js
app.set('port', process.env.PORT || 3000);
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'pug');

app.use(morgan('dev'));
```

- 순서는 미들웨어 위에 설정!



index.pug 파일 생성 후 index.js 수정

``` pug
// views/index.pug
doctype html 
html 
    head 
        title= title 
    body 
        h1 hello! 
```

```javascript
// routes/index.js
const express = require('express');
const path = require('path');
const router = express.Router();

router.get('/', (req, res, next) =>{
    res.render('index')
})

module.exports = router;
```



### render 시 데이터 넘겨주기

- 데이터 넘길 때 두번 째 파라미터, 객체로 넘겨줌

``` javascript
router.get('/', (req, res, next) =>{
    res.render('index', { msg: 'hello!!!!!'}) 
})
```

```pug
doctype html 
html 
    head 
        title Hello 
    body 
        h1= msg
        h1 msg: #{msg} 
```

- `#{파라미터}` 사용 가능



### 배열 넘겨주기

``` javascript
router.get('/', (req, res, next) =>{
    res.render('index', { arr: ['a','b','c']})
})
```

``` pug
doctype html 
html 
    head 
        title Hello 
    body 
        ul 
            each item in arr
                li=item

        table
            tr
                th Index 
                th Item 
            each item, index in arr
                tr 
                    td=index 
                    td=item
```



### 이미지 넘겨주기 + 조건문 사용

``` js
router.get('/', (req, res, next) =>{
    res.render('index', { user : { id:'name', img: 'https://upload.wikimedia.org/wikipedia/commons/4/45/NuxtJS_Logo.png'}}) 
})
```

```pug
doctype html 
html 
    head 
        title Hello 
        style. 
            p {
                color: blue;
            }
    body 
        p hello #{user.id}

        if user.img
        	p path: #{user.img}
            img(src=user.img)
```

- 애트리뷰트에서 쓸 때는 `#{}` 안해도 된다는 차이 기억
- `user.img` 가 있을 때만 img 표시



### 레이아웃 ( block 사용 )

```pug
// layout.pug

doctype html 
html 
    head      
    body 
        h1 HELLO
        block content 
        h1 FOOTER
```

```pug
// index.pug

extends layout 

block content
    p hello #{user.id}

        if user.img
            p path: #{user.img}
            img(src=user.img)
```



## nunjucks

- 확장자는 html 또는 njk
- `npm i nunjucks`

```javascript
// app.js

const nunjucks = require('nunjucks');

// ..

app.set('view engine', 'html');
nunjucks.configure('views', {
    express: app,
    watch:true,
});
```

- 변수 사용 `{{ user.img }}`
