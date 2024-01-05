---
title: sequelize 모듈
author: Psmin
data: 2023-05-24 17:26:41 +0900
categories: [Knowledge, Nodejs]
tags: [sequelize]
---

## ORM (Object-Relational Mapping)

> Object-Relational Mapping, 객체-관계 매핑

ORM은 객체와 RDB의 데이터를 자동으로 매핑(연결)해주는 것을 말합니다.

예를 들면, 객체 지향 언어에서는 모델을 정의할 때 **Class를 사용**하고, RDB에선 **Table을 사용**합니다.

이 떄, <u>객체 지향 언어로 된 Class를 RDB의 Table와 연결</u>시켜주는 것으로 SQL문이 아닌 객체 지향 코드로 작성할 수 있습니다.

- **장점**

  - 객체 지향적인 코드로 작성하므로 **직관적**
  - **재사용 및 유지보수**의 편리성 증가

---

## Sequelize

Sequelize는 **Postgres, MySQL, MariaDB, SQLite, Microsoft SQL Server**를 지원하는 **Promise 패턴 기반의 Node.js ORM**입니다

> Spring의 JPA/Hibernate, Django의 Django ORM

ORM의 특징은 특정 DB에 종속되지 않는다는 것입니다.

즉, DB와 커넥션만 연결되면 어떤 DB를 사용하던지 상관없이 동일한 메서드로 쿼리 수행이 가능합니다.

- **설치**

  ```js
  npm i sequelize, sequelize-cli
  ```

---

## sequelize과 Mysql 연결

Sequelize를 사용하기 위해서는 Postgresql , MySQL , MSSQL, SQLite 등의 RDB가 시스템에 설치되어있어야 합니다.

Mysql을 연결해보겠습니다.

```js
npm i mysql2
```

---

### sequelize-cli

sequelize-cli 모듈은 sequelize를 조금 더 효율적으로 사용하기 위한 **뼈대를 구축**해줍니다.

```js
npm i sequelize-cli
```

설치가 완료되면 `sequelize 명령어`를 실행할 수 있습니다.

```console
sequelize init
```

명령 실행하면 **config**, **models**, **migrations**, **seeders** 폴더가 생성됩니다.

---

### models/index.js

sequelize-cli가 자동으로 생성해주는 코드는 에러가 발생하기도 하고 불필요한 부분이 많아 간단하게 새로 작성해줍니다.

> 참고 글 : [Sequelize - Getting Started](https://sequelize.org/docs/v6/getting-started/)

```js
const Sequelize = require("sequelize");
const env = process.env.NODE_ENV || "development"; // 지정된 환경변수가 없으면 'development'로 지정

const config = require("../config/config.json")[env];
// config.json 파일에 여러 변수명으로 객체를 생성하여 DB 설정을 관리합니다.
// config객체의 env변수(development)키 의 객체값들을 불러옵니다.
// 즉, 데이터베이스 설정을 불러오는 것

const db = {};

// new Sequelize를 통해 MySQL 연결 객체를 생성합니다.
const sequelize = new Sequelize(
  config.database,
  config.username,
  config.password,
  config
);

// 연결객체를 나중에 재사용하기 위해 db.sequelize에 넣어줍니다.
db.sequelize = sequelize;

module.exports = db;
```

---

### config/config.json

DB 설정을 관리하는 파일입니다.

사용할 DB 정보를 입력해줍니다.

```json
{
  "development": {
    "username": "root",
    "password": null,
    "database": "database_production",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "test": {
    "username": "root",
    "password": null,
    "database": "database_test",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "production": {
    "username": "root",
    "password": null,
    "database": "database_production",
    "host": "127.0.0.1",
    "dialect": "mysql"
  }
}
```

---

### express 와 sequelize 연결

app.js를 생성하여 **express와 sequelize를 연결**해보겠습니다.

```js
// app.js

const express = require("express");

// index.js에 있는 db.sequelize 객체 모듈을 구조분해로 불러온다.
const { sequelize } = require("./models");
const app = express();

app.set("port", process.env.PORT || 3000);

// sync() 를 통해 데이터베이스에 동기화 합니다.
try {
  await db.sequelize.sync({ force: false });
  console.log("데이터베이스 연결됨.");
} catch (err) {
  console.log(err);
}

app.use(express.json()); // json 파싱
app.use(express.urlencoded({ extended: false })); // uri 파싱

app.get("/", (req, res) => {
  res.json({ message: "hello world" });
});

// 서버 실행
app.listen(app.get("port"), () => {
  console.log(app.get("port"), "번 포트에서 대기 중");
});
```

---

### 연결 테스트

![sequelize-connect](/assets/img/sequelize-connect.png){: .w-80}

에러 없이 잘 연결된 것을 볼 수 있습니다.

---

## 모델 정의하기

`sequlize.define()`을 이용해 User 테이블을 생성해보겠습니다.

define 메서드는 **Sequelize에서 모델을 정의**하는 데 사용되는 메서드입니다.

이를 통해 Sequelize는 DB의 테이블과 **모델을 매핑**하고, **모델을 생성**하며, 해당 모델을 사용하여 **DB와 상호작용**할 수 있습니다.

> 형태 : define(modelName: string, attributes: object, options: object)

- **주요 매개변수**

  - <kbd>modelName</kbd> : 모델의 이름을 지정합니다.

    이 이름은 Sequelize에서 사용되며, 데이터베이스에 영향을 주지 않습니다.

  - <kbd>attributes</kbd> : 테이블의 열(컬럼)을 정의하는 객체입니다.

    각 열은 이름, 데이터 유형, 제약 조건 등을 포함합니다.

  - <kbd>options</kbd> : 모델의 추가적인 설정을 지정하는 객체입니다.

    - **timestamps** : true 설정 시, created_at, updated_at 컬럼이 생성되며 데이터의 생성, 수정 시 시간이 자동으로 입력됩니다.
    - **paranoid** : true 설정 시, deletedAt 컬럼이 생성되며 지운 시각이 기록됩니다.

    - **underscored** : true 설정 시, 카멜케이스 대신 스네이크케이스를 사용

    - **freezeTableName** : true 설정 시, 모델이름 변환 X, 지정한 이름 사용
    - **tableName** : 테이블 이름 명시적 설정

    - **charset** : 인코딩

---

### users.js 파일

**models** 폴더에 **_users.js_** 파일을 생성합니다.

**class로 모듈화**하고 **import/export를 사용**해보겠습니다.

```js
// users.js
import { DataTypes } from "sequelize";

class User {
  static define(sequelize) {
    sequelize.define(
      "User", // ModelName
      {
        user_id: {
          type: DataTypes.INTEGER,
          unique: true,
          allowNull: false,
          // validate
        },
        password: {
          type: DataTypes.STRING(30),
          allowNull: false,
        },
      }, // attributes
      {
        timestamps: false, // true : created_at, updated_at 컬럼이 생성되며 데이터의 생성, 수정 시 시간이 자동으로 입력됩니다.
        paranoid: false /* true : deletedAt 컬럼이 생성되며 지운 시각이 기록됩니다. */,
        underscored: true, // true : 카멜케이스 대신 스네이크케이스를 사용
        freezeTableName: true, // 모델이름 변환 X, 지정한 이름 사용
        tableName: "users", // 테이블 이름 명시적 설정
      } // options
    );
  }
}

export default User;
```

---

### sequelize의 자료형 (DataTypes)

sequelize의 자료형은 MySQL과 다르므로 확인 후 사용해야합니다.

> 참고 글 : [Sequelize - Datetypes](https://sequelize.org/api/v6/variable/#static-variable-DataTypes)

![sequelize-datatypes](/assets/img/sequelize-datatypes.png){: .w-80}

---

### models/index.js 수정

**_index.js_** 파일에 **_users.js_**을 import한 후 연결해보겠습니다.

```js
const Sequelize = require("sequelize");
const env = process.env.NODE_ENV || "development"; // 지정된 환경변수가 없으면 'development'로 지정

// ************* 추가된 부분 ***************
// 클래스를 불러온다.
import User from "./users.js";
// ****************************************

const config = require("../config/config.json")[env];
// config.json 파일에 여러 변수명으로 객체를 생성하여 DB 설정을 관리합니다.
// config객체의 env변수(development)키 의 객체값들을 불러옵니다.
// 즉, 데이터베이스 설정을 불러오는 것

const db = {};

// new Sequelize를 통해 MySQL 연결 객체를 생성합니다.
const sequelize = new Sequelize(
  config.database,
  config.username,
  config.password,
  config
);

// 연결객체를 나중에 재사용하기 위해 db.sequelize에 넣어줍니다.
db.sequelize = sequelize;

// ************* 추가된 부분 ***************
// 모델 클래스를 넣어줍니다.
db.User = User;

// 모델과 테이블 종합적인 연결이 설정된다.
User.define(sequelize);
// ****************************************

module.exports = db;
```

---

### 연결 테스트

![sequelize-create-table](/assets/img/sequelize-create-table-01.png){: .w-80}

![sequelize-create-table](/assets/img/sequelize-create-table-02.png){: .w-80}

에러 없이 users 테이블이 잘 생성된 것을 확인할 수 있습니다.

---

## sequelize.sync() 메서드

sync 메서드는
