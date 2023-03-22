## 시퀄라이즈(Sequelize) 쿼리



### 데이터 삽입

`INSERT INTO 테이블명 (name, age) VALUES ('wonny', 16);`

```javascript
const User = require('../medels');
User.create({
  name :'wonny',
  age : 16
})
```



### 모든 데이터 조회

`SELECT * FROM 테이블명;`

```javascript
const User = require('../medels');
User.findAll({})
```



### 데이터 하나만 조회

`SELECT * FROM 테이블명 LIMIT 1;`

```javascript
const User = require('../medels');
User.findOne({})
```



### 원하는 컬럼만 조회

`SELECT name FROM 테이블명;`

```javascript
const User = require('../medels');
User.findAll({
  attributes:['name']
})
```



### 테이블 정렬

`SELECT id, name FROM 테이블명 ORDER BY age DESC;`

```javascript
const User = require('../medels');
User.findAll({
  attributes:['id', 'name'],
  order:[['age', 'DESC']],
})
```



### 컬럼 수정(UPDATE)

`UPDATE 테이블명 SET name = '이름' WHERE id = 1;`

```javascript
const User = require('../medels');
User.update({
  name : '이름',
  }, {
  where: { id : 1 }
})
```



### 컬럼 삭제(DELETE)

`DELETE FROM 테이블명 WHERE id = 1;`

```javascript
const User = require('../medels');
User.destory({
  where: { id : 1 }
})
```