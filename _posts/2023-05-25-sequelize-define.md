---
title: sequelize 모듈 - define 메서드
author: Psmin
data: 2023-05-25 22:41:57 +0900
categories: [Knowledge, Nodejs]
tags: [sequelize, define]
---

## define으로 모델 정의

`sequlize.define()`을 이용해 User 테이블을 생성해보겠습니다.

define 메서드는 **Sequelize에서 모델을 정의**하는 데 사용되는 메서드입니다.

이를 통해 Sequelize는 DB의 테이블과 **모델을 매핑**하고, **모델을 생성**하며, 해당 모델을 사용하여 **DB와 상호작용**할 수 있습니다.

> 형태 : define(modelName: string, attributes: object, options: object)

- **주요 매개변수**

  - <kbd>modelName</kbd> : <u>모델의 이름을 지정</u>합니다.

    이 이름은 Sequelize에서 사용되며, 데이터베이스에 영향을 주지 않습니다.

  - <kbd>attributes</kbd> : <u>테이블의 열(컬럼)을 정의하는 객체</u>입니다.

    각 열은 이름, 데이터 유형, 제약 조건 등을 포함합니다.

    - **type** : 데이터 타입 명시
    - **primaryKey** : 기본 키 설정
    - **allowNull** : Null 허용 여부
    - **unique** : 중복 값 저장 여부
    - **autoIncrement** : Auto Increase 기능 사용 여부
    - **defaultValue** : 기본 값 명시
    - **validate** : 유효성 검사 옵션

  - <kbd>options</kbd> : <u>모델의 추가적인 설정을 지정하는 객체</u>입니다.

    - **tableName** : 테이블 이름 명시적 설정
    - **freezeTableName** : true 설정 시, 모델이름 변환 X, 지정한 이름 사용
    - **underscored** : true 설정 시, 카멜케이스 대신 스네이크케이스를 사용
    - **timestamps** : true 설정 시, <u>created_at, updated_at 컬럼이 생성되며 데이터의 생성, 수정 시 시간이 자동으로 입력</u>됩니다.
    - **paranoid** : true 설정 시, <u>deletedAt 컬럼이 생성되며 지운 시각이 입력</u>됩니다.
    - **charset** : 인코딩

---

### models/users.js

models 폴더에 **_users.js_** 파일을 생성합니다.

**class로 모듈화**하고 **import/export를 활용해 사용**해보겠습니다.

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
          primaryKey: true,
          autoIncrement: true,
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

### models/index.js 수정

**_index.js_** 파일에 **_users.js_** 파일을 import한 후 연결해보겠습니다.

```js
const Sequelize = require("sequelize");
const env = process.env.NODE_ENV || "development"; // 지정된 환경변수가 없으면 'development'로 지정

// 클래스를 불러온다.
import User from "./users.js";

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

// 모델 클래스를 넣어줍니다.
db.User = User;

// 모델과 테이블 종합적인 연결이 설정된다.
User.define(sequelize);

module.exports = db;
```

---

## app.js

`sequelize.sync()` 를 이용하면 자동으로 모든 모델을 동기화할 수 있습니다.

**_app.js_** 파일에 **_models/index.js_** 파일을 import한 후 연결해보겠습니다.

```js
import express from "express";
import "dotenv/config";

import db from "./models/index.js";
const app = express();

app.set("port", process.env.PORT || 8000);

try {
  await db.sequelize.sync();
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

## 연결 테스트

![sequelize-create-table-define-01](/assets/img/sequelize-create-table-define-01.png){: .w-80}

![sequelize-create-table-define-02](/assets/img/sequelize-create-table-define-02.png){: .w-80}

에러 없이 users 테이블이 잘 생성된 것을 확인할 수 있습니다.
