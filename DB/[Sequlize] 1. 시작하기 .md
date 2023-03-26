```
npm install sequelize sequelize-cli pg pg-hstore dotenv
```



### .sequelizerc 파일 생성

```
const path = require('path')

module.exports = {
  'config': path.resolve('./database/config', 'config.js'),
  'models-path': path.resolve('./database/models'),
  'migrations-path': path.resolve('./database/migrations'),
  'seeders-path': path.resolve('./database/seeders')
}
```

- '`config'`: PostgreDB 서버와 연결할 connection string를 설정하는 폴더를 sequelize-cli가 생성하도록 해줌
  `'models-path'`: 테이블의 스키마를 정의해준 디렉토리. `sequelize-cli`가 이 파일을 바탕으로 마이그레이션 파일을 생성.
  `'migrations-path'`: 실제로 cli가 DB서버에 테이블을 만들도록 도와주는 디렉토리.
  `'seeders-path'`: 실제 데이터들을 넣는 폴더. cli가 데이터들을 보고 DB의 테이블에 데이터들을 넣음.




### 구조 생성

```
npx sequelize init
```

- `.sequelizerc `에 적혀있는 위치에 생성됨



### models/index.js

```js
const Sequelize = require("sequelize");

const env = process.env.NODE_ENV || "development";
const config = require(__dirname + "/../config/config.json")[env];
const db = {};

const sequelize = new Sequelize(
  config.database,
  config.username,
  config.password,
  config
);

db.sequelize = sequelize;

module.exports = db;
```

- 최소한의 코드



### 모델 생성

```
sequelize model:generate --name User --attributes user_id:string,user_name:string
```

- 인자간 띄어쓰기 하지말것



### models/user.js 수정

```
npx sequelize db:migrate
```

- 실행할 때는 루트폴더에서 진행!!! ⭐



###  config.js

``` js
require('dotenv').config();
const env = process.env;

const development = {
  username: env.POSTGRES_USERNAME,
  password: env.POSTGRES_PASSWORD,
  database: env.POSTGRES_DATABASE,
  host: env.POSTGRES_HOST,
  dialect: "postgres",
  //port: env.POSTGRES_PORT
};

const production = {
  username: env.POSTGRES_USERNAME,
  password: env.POSTGRES_PASSWORD,
  database: env.POSTGRES_DATABASE,
  host: env.POSTGRES_HOST,
  dialect: "postgres",
  //port: env.POSTGRES_PORT
};

const test = {
  username: env.POSTGRES_USERNAME,
  password: env.POSTGRES_PASSWORD,
  database: env.POSTGRES_DATABASE_TEST,
  host: env.postgres,
  dialect: "POSTGRES",
  //port: env.POSTGRES_PORT
};

module.exports = { development, production, test };
```



### .env 파일 (root폴더에서 생성!!!⭐)

```
POSTGRES_USERNAME= (사용자 이름)
POSTGRES_PASSWORD= (비번)
POSTGRES_DATABASE= (데이터베이스 이름)
POSTGRES_HOST=127.0.0.1
```



### app.js - sync()  -> 모델 생성

- `app.js` 상단에 작성

```js
// ... 
const models = require("./models/index.js");

models.sequelize.sync().then( () => {
    console.log(" DB 연결 성공");
}).catch(err => {
    console.log("연결 실패");
    console.log(err);
})
```

