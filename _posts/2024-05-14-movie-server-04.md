---
title: Server - 04
author: Psmin
data: 2024-05-14 16:26:44 +0900
categories: [Project, Movie Story]
tags: [JWT, session]
---

# 로그인, 회원가입 기능 구현 (Server)

---

## DB 설계

이전에 설계한 member 테이블을 삭제하고 새로 user 테이블을 정의해보겠습니다.

[이전 모델링 포스팅](https://psmin1994.github.io/posts/movie-01/)

---

### user 테이블 설계

먼저 회원 정보 관련 요구사항을 추가 정리합니다.

1. **회원** 정보는 **아이디**, **비밀번호**, **이름** 등의 정보가 유지되어야합니다.

> validation 내용 추가 예정

1. 아이디는 4 ~ 12 글자이어야 하고 영어 대소문자, 숫자로만 구성되어야합니다.

2. 비밀번호는 4 ~ 12 글자이어야 하고 영어, 숫자, 특수문자(!@#$%^&\*)가 하나 이상 포함되어야합니다.

---

요구사항 1번에서 **회원 엔티티**의 속성으로 **아이디**, **비밀번호**, **이름**를 얻을 수 있습니다.

---

DB 명명 규칙에 따라 테이블 명세서를 작성합니다.

> 참고 글 : [DB 명명 규칙](https://psmin1994.github.io/posts/db-naming/)

![user-table-specs](/assets/img/user-table-specs.png)

---

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

router.post("/login", ctrl.login); // 유저 로그인
router.post("/register", ctrl.register); // 유저 회원가입

export default router;
```

---

### controller

controllers 폴더에 `user.controller.js` 파일을 생성한 후 작성하겠습니다.

```js
import userStorage from "../models/user.model.js";

export default {
  login: async (req, res, next) => {
    let response = {};

    try {
      let { id, password } = req.body;

      const user = await userStorage.getUserInfo(id);

      // 아이디 없음
      if (!user) {
        response = {
          success: false,
          msg: "아이디가 존재하지 않습니다",
        };
      } else {
        if (password === user.password) {
          response = {
            success: true,
            msg: "로그인 성공",
          };
        } else {
          response = {
            success: false,
            msg: "비밀번호가 틀렸습니다",
          };
        }
      }

      res.json(response);
    } catch (err) {
      next(err);
    }
  },

  register: async (req, res, next) => {
    let response = {};

    try {
      let { id, password } = req.body;

      const checked = await userStorage.checkUser(id);

      if (checked) {
        response = { success: false, msg: "이미 존재하는 아이디입니다" };
      } else {
        // DB에 새로운 사용자 등록
        await userStorage.createUser(req.body);

        response = { success: true, msg: "회원가입 완료." };
      }

      res.json(response);
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
import conn from "../config/dbPool.js";

class userStorage {
  static async checkUser(id) {
    try {
      await conn.beginTransaction();

      const [rows] = await conn.query(
        "select EXISTS (SELECT id FROM user WHERE id = ?) AS isExist",
        [id]
      );

      await conn.commit();

      return rows[0].isExist;
    } catch (err) {
      await conn.rollback();

      throw err;
    } finally {
      conn.release();
    }
  }

  static async getUserInfo(id) {
    try {
      await conn.beginTransaction();

      const [rows] = await conn.query("SELECT * FROM user WHERE id = ?", [id]);

      await conn.commit();

      return rows[0];
    } catch (err) {
      await conn.rollback();

      throw err;
    } finally {
      conn.release();
    }
  }

  static async createUser(userInfo) {
    try {
      await conn.beginTransaction();

      const [rows] = await conn.query(
        "INSERT INTO user (id, password, name) VALUES (?, ?, ?)",
        [userInfo.id, userInfo.password, userInfo.name]
      );

      await conn.commit();

      return rows[0];
    } catch (err) {
      await conn.rollback();

      throw err;
    } finally {
      conn.release();
    }
  }
}

export default userStorage;
```
