---
title: 영화 정보 제공 사이트 (Server) - 04
author: Psmin
data: 2024-02-14 16:26:44 +0900
categories: [Project, Movie Story]
tags: [JWT, session]
---

# 로그인, 회원가입 기능 구현 (Server)

---

## DB 설계

이전에 설계한 member 테이블을 삭제하고 새로 user 테이블을 정의해보겠습니다.

[이전 모델링 포스팅](https://psmin1994.github.io/posts/movie-01/)을 기반으로 설계 과정은 간단하게 진행하겠습니다.

---

## user 테이블 설계

먼저 회원 정보 관련 요구사항을 추가 정리합니다.

> validation 내용 추가

1. **회원** 정보는 **아이디**, **비밀번호**, **이름** 등의 정보가 유지되어야합니다.

2. 아이디는 8 ~ 12 글자이어야 하고 영어 대소문자, 숫자로만 구성되어야합니다.

3. 비밀번호는 8 ~ 15 글자이어야 하고 영어, 숫자, 문자가 하나 이상 포함되어야합니다.

---

요구사항에서 명사들을 추출해 개체와 속성을 작성합니다.

- 요구사항 1번에서 **회원 엔티티**의 속성으로 **아이디**, **비밀번호**, **이름**를 얻을 수 있습니다.

---

### 명세서 작성

DB 명명 규칙에 따라 테이블 명세서를 작성합니다.

> 참고 글 : [DB 명명 규칙](https://psmin1994.github.io/posts/db-naming/)

![user-table-specs](/assets/img/user-table-specs.png)

---

### 물리적 데이터 모델링

이제 Workbench에 테이블 명세서를 바탕으로 ER 다이어그램을 수정합니다.

> 참고 글 : [물리적 데이터 모델링](https://psmin1994.github.io/posts/physical-modeling/)

---

## express 서버 API 구현

---

### app.js

`app.js`파일에 user 라우터를 추가합니다.

```js
import express from "express";
import bodyParser from "body-parser";
import cors from "cors";
import morganMiddleware from "./src/middleware/morgan.js";
import movieRouter from "./src/routes/movie.route.js";
import userRouter from "./src/routes/user.route.js";

const app = express();

...

// router
app.use("/movie", movieRouter);
app.use("/user", userRouter);

...

export default app;
```

---

### router

routes 폴더에 `user.route.js` 파일을 생성한 후 express router를 작성합니다.

```js
import express from "express";
import ctrl from "../controllers/user.controller.js";

const router = express.Router();

router.post("/login", ctrl.login); // 유저 로그인인

export default router;
```

---

### controller

controllers 폴더에 `user.controller.js` 파일을 생성한 후 작성하겠습니다.

```js
import userStorage from "../models/user.model.js";

export default {
  login: async (req, res, next) => {
    try {
      let { id, password } = req.body;

      const data = await userStorage.getUserId(id, password);

      res.json(data);
    } catch (err) {
      next(err);
    }
  },
};
```

---

### model

models 폴더에 `user.model.js` 파일을 생성한 후 작성해보겠습니다.

```js
import getConnection from "../config/dbPool.js";

class userStorage {
  static async getUserId(id, pwd) {
    try {
      const conn = await getConnection();

      const [rows] = await conn.query(
        "SELECT user_id FROM user WHERE id = ? & password = ?",
        [id, pwd]
      );

      conn.release();

      return rows;
    } catch (err) {
      throw err;
    }
  }
}

export default userStorage;
```
