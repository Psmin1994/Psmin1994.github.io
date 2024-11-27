---
title: 영화 정보 제공 사이트 (Server) - 07
author: Psmin
data: 2024-02-23 18:14:51 +0900
categories: [Project, Movie Story]
tags: [passport]
---

# passport 모듈 적용

추가적으로 SSO를 사용할 예정이므로 사용자 인증 과정에 passport 모듈을 적용하겠습니다.

> 참고글 : [passport](https://psmin1994.github.io/posts/passport/), [로그인 페이지 만들기 - 08](https://psmin1994.github.io/posts/login-08/)

---

## index.js 작성

`passport` 폴더를 만들고 `index.js` 파일을 작성합니다.

```js
import local from "./localStrategy.js";

export default () => {
  // 로컬 로그인
  local();

  // 추후 여러 SSO 추가 예정
};
```

---

## localStrategy.js 작성

```js
import passport from "passport";
import { Strategy as LocalStrategy } from "passport-local";
import { userPasswordVerify } from "../utils/crypto.util.js";

import UserStorage from "../models/user.model.js";

export default () => {
  passport.use(
    "local",
    new LocalStrategy(
      // new LocalStrategy의 첫번째 인자에 객체 형태로 여러 옵션 지정
      // usernameField: 아이디 정보가 들어있는 필드명 지정 (default : username)
      // passwordField: 비밀번호 정보가 들어있는 필드명 지정 (default : password)
      // session : 세션 저장 여부 (default : true)
      // passReqToCallback : 요청 객체(req)를 콜백에 전달 (default : false)
      {
        usernameField: "id",
        passwordField: "password",
        session: false,
      },
      async (id, password, done) => {
        try {
          let user = await UserStorage.getUserInfo(id);

          if (!user) {
            return done(null, false, {
              success: false,
              msg: "가입되지 않은 회원입니다.",
            });
          }

          let checkedPassword = await userPasswordVerify(
            password,
            user.password
          );

          if (checkedPassword) {
            return done(null, user);
          }

          done(null, false, {
            success: false,
            msg: "비밀번호가 틀렸습니다",
          });
        } catch (err) {
          done(err);
        }
      }
    )
  );
};
```

---

## Controller 파일 수정

`user.controller.js` 파일에서 login 인증에 passport 모듈을 적용합니다.

```js
import passport from "passport";
import userStorage from "../models/user.model.js";
import { createPasswordAndSalt } from "../utils/crypto.util.js";

export default {
  login: (req, res, next) => {
    passport.authenticate("local", async (err, user, response) => {
      if (err) return next(err);

      if (!user) return res.json(response);

      res.status(200).json({
        success: true,
        msg: "로그인 성공",
      });
    })(req, res, next); // 미들웨어 내의 미드뤠어는 (req, res, next)를 붙여줍니다.
  },

  register: async (req, res, next) => {
    ...
  }
}
```
