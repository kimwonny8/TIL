## 시퀄라이즈(Sequelize) 쿼리

**1. Row를 생성하는 쿼리**

```javascript
SQL문 : INSERT INTO 테이블명 (name, age) VALUES ('ji', 20);
//시퀄라이즈
const { User } = require('../medels');
User.create({
  name :'ji',
  age : 20
})
```

**2. 테이블의 모든 데이터를 조회하는 쿼리**

```javascript
SQL문 : SELECT * FROM 테이블명;
//시퀄라이즈
const { User } = require('../medels');
User.findAll({})
```

**3. 테이블의 데이터를 하나만 조회하는 쿼리**

```javascript
SQL문 : SELECT * FROM 테이블명 LIMIT 1;
//시퀄라이즈
const { User } = require('../medels');
User.findOne({})
```

**4. 원하는 컬럼만 가져오는 쿼리**

```javascript
SQL문 : SELECT name FROM 테이블명;
//시퀄라이즈
const { User } = require('../medels');
User.findAll({
  attributes:['name']
})
```

**5. 테이블 정렬 쿼리**

```javascript
SQL문 : SELECT id, name FROM 테이블명 ORDER BY age DESC;
//시퀄라이즈
const { User } = require('../medels');
User.findAll({
  attributes:['id', 'name'],
  order:[['age', 'DESC']],
})
```

**6. Row 수정하는 쿼리**

```javascript
SQL문 : UPDATE 테이블명 SET name = '바꿀 이름' WHERE id = 2;
//시퀄라이즈
const { User } = require('../medels');
User.update({
  name : '바꿀 이름',
  }, {
  where: { id : 2 }
})
```

**6. Row 삭제하는 쿼리**

```javascript
SQL문 : DELETE FROM 테이블명 WHERE id = 2;
//시퀄라이즈
const { User } = require('../medels');
User.destory({
  where: { id : 2 }
})
```