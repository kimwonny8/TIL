## 테이블 복수화 막기

```js
  freezeTableName: true,
```

```js
'use strict';
const {
  Model
} = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class member extends Model {
    /**
     * Helper method for defining associations.
     * This method is not a part of Sequelize lifecycle.
     * The `models/index` file will call this method automatically.
     */
    static associate(models) {
      // define association here
    }
  }
  member.init({
    m_id: DataTypes.STRING,
    m_pw: DataTypes.STRING,
    m_name: DataTypes.STRING,
    m_directory: DataTypes.STRING
  }, {
    sequelize,
    modelName: 'member',
    freezeTableName: true,
  });
  return member;
};
```



## createAt 등 없애기

``` js
timestamps: false,
```

```js
'use strict';
const {
  Model
} = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class member extends Model {
    /**
     * Helper method for defining associations.
     * This method is not a part of Sequelize lifecycle.
     * The `models/index` file will call this method automatically.
     */
    static associate(models) {
      // define association here
    }
  }
  member.init({
    m_id: DataTypes.STRING,
    m_pw: DataTypes.STRING,
    m_name: DataTypes.STRING,
    m_directory: DataTypes.STRING
  }, {
    timestamps: false,
    sequelize,
    modelName: 'member',
    freezeTableName: true,
  });
  return member;
};
```



## id 컬럼 없는데 있다고 뜰 때

```js
 member.removeAttribute('id');
```

```js
module.exports = (sequelize, DataTypes) => {
  const member = sequelize.define('member', {
    m_id: DataTypes.STRING,
    m_pw: DataTypes.STRING,
    m_name: DataTypes.STRING,
    m_directory: DataTypes.STRING
  }, {  
    timestamps: false,
    sequelize,
    modelName: 'member',
    freezeTableName: true,
  });

  member.removeAttribute('id');

  return member;
}
```

