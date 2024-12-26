---
title: Server - 09
author: Psmin
data: 2024-02-27 03:23:21 +0900
categories: [Project, Movie Story]
tags: [passport, oauth]
---

# OAuth 2.0 인증을 구현해보자.

Naver 계정을 통해 OAuth 인증을 구현해보겠습니다.

> 참고글 : [OAuth](https://psmin1994.github.io/posts/oauth/)

---

## OAuth 서비스 등록 (Naver)

OAuth 2.0 로그인을 사용하기 위해서는 이용하고자 하는 **Resource Server**에 자신의 서비스(어플리케이션)을 등록해야합니다.

[Naver Developers 페이지](https://developers.naver.com/main/)의 _'Application - 어플리케이션 등록'_ 으로 이동합니다.

![naver-oauth-01](/assets/img/naver-oauth-01.png)

어플리케이션 이름을 작성하고 **사용 API**에서 **네이버 로그인**을 선택한 후 **Scope(가져올 데이터)를 설정**합니다.

![naver-oauth-02](/assets/img/naver-oauth-02.png)

**환경 추가**에서 **PC 웹**을 선택하고 **서비스 URL**과 **Callback URL**을 작성하고 등록합니다.

> 서비스 URL은 GitHub OAuth 인증 과정 자체에 직접적인 영향을 미치지 않습니다.  
> 배포 후에는 실제 클라이언트 주소로 변경하는 것이 좋습니다.

![naver-oauth-03](/assets/img/naver-oauth-03.png)

_'Application - 내 어플리케이션'_ 에서 **Client ID**와 **Client Secret**을 확인할 수 있습니다.

> ? Callback URL, Client ID, Client Secret를 기억해둡니다.

---

## oauth_user 테이블 설계

**다양한 인증 제공자(Google, Facebook, Naver 등)를 관리**할 수 있도록 **유연하고 확장 가능한 구조**로 설계합니다.

> 모델링 과정은 생략하겠습니다.

![oauth-table](/assets/img/oauth-table.png)

---

## auth.model.js 작성

OAuth를 통해 제공받은 데이터를 관리할 model 파일을 구현합니다.

```js
import pool from "../config/dbPool.js";

class authStorage {
  // oauth_user 테이블에 사용자 id로 저장된 데이터가 있는 지 확인
  static async getOauthUserById(id) {
    const conn = await pool.getConnection();

    try {
      await conn.beginTransaction();

      const [rows] = await conn.query(
        "SELECT * FROM oauth_user WHERE provider_user_id = ?",
        [id]
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

  // 신규 oauth 사용자 데이터 저장
  static async createOauthUser(userInfo) {
    const conn = await pool.getConnection();

    try {
      await conn.beginTransaction();

      const row = await conn.query(
        "INSERT INTO oauth_user (provider, provider_user_id, nickname, email ,access_token,refresh_token) VALUES (?, ?, ?, ?, ?, ?)",
        [
          userInfo.provider,
          userInfo.provider_user_id,
          userInfo.nickname,
          userInfo.email,
          userInfo.accessToken,
          userInfo.refreshToken,
        ]
      );

      await conn.commit();

      return row[0].insertId;
    } catch (err) {
      await conn.rollback();

      throw err;
    } finally {
      conn.release();
    }
  }

  // 기존 oauth 사용자가 oauth 시도 할 시, 갱신
  // update_at 데이터는 UPDATE 시도 시 자동 갱신
  static async UpdateOauthUser(oauthUser) {
    const conn = await pool.getConnection();

    try {
      await conn.beginTransaction();

      const row = await conn.query(
        "UPDATE oauth_user SET access_token = ?, refresh_token = ? WHERE provider_user_id = ?",
        [
          oauthUser.accessToken,
          oauthUser.refreshToken,
          oauthUser.provider_user_id,
        ]
      );

      await conn.commit();

      return row;
    } catch (err) {
      await conn.rollback();

      throw err;
    } finally {
      conn.release();
    }
  }
}

export default authStorage;
```

---

## passport-naver-v2 모듈

**Passport**를 사용하여 네이버 로그인을 구현할 수 있도록 지원하는 인증 전략입니다.

네이버의 OAuth 2.0 API를 활용하여 사용자를 인증하고 사용자 정보를 가져오는 데 사용됩니다.

- **설치**

  ```
  npm i passport-naver-v2
  ```

---

## index.js

`index.js`에 구현한 네이버 전략을 등록합니다.

```js
import passport from "passport";
import local from "./localStrategy.js";
import jwt from "./jwtStrategy.js";
import naver from "./naverStrategy.js";

// 로컬 로그인
local(passport);

// JWT 검증
jwt(passport);

// naver 로그인
naver(passport);

export default passport;
```

---

### 데이터 출력 확인

passport 폴더에 `naverStrategy.js`을 생성하고 네이버 전략을 구현합니다.

> 위에서 기억한 Client ID, Client Secret, Callback URL은 환경변수로 저장한 후 가져옵니다.

```js
import { Strategy as NaverStrategy } from "passport-naver-v2";
import authStorage from "../models/auth.model.js";

export default (passport) => {
  passport.use(
    "naver",
    new NaverStrategy(
      {
        clientID: process.env.NAVER_CLIENT_ID,
        clientSecret: process.env.NAVER_CLIENT_SECRET,
        callbackURL: process.env.NAVER_CALLBACK_URL,
        svcType: 0,
      },
      // accessToken : 추가적인 Naver API 사용 시 필요
      async (accessToken, refreshToken, profile, done) => {
        // 사용자 인증 성공 시 호출
        console.log(profile); // 가져온 데이터 출력해보기

        return done(null, profile);
      }
    )
  );
};
```

---

`console.log(profile)`로 출력된 값을 살펴보겠습니다.

**Resource Owner**가 허용한 데이터가 출력되는 것을 볼 수 있습니다.

```json
// profile 객체
{
  "provider": "naver",
  "id": "cPJ5IXic2cC-OlxKWxoJOAOfxWFETlFxnLWEI8r_yiE",
  "nickname": undefined,
  "profileImage": undefined,
  "age": undefined,
  "gender": undefined,
  "email": "Your Email",
  "mobile": undefined,
  "mobileE164": undefined,
  "name": "Your Name",
  "birthday": undefined,
  "birthYear": undefined,
  "_json": { ... } // 네이버가 제공한 원본 응답 데이터
}
```

---

## Strategy 작성

```js
import { Strategy as NaverStrategy } from "passport-naver-v2";
import authStorage from "../models/auth.model.js";

export default (passport) => {
  passport.use(
    "naver",
    new NaverStrategy(
      {
        clientID: process.env.NAVER_CLIENT_ID,
        clientSecret: process.env.NAVER_CLIENT_SECRET,
        callbackURL: process.env.NAVER_CALLBACK_URL,
        svcType: 0,
      },
      // accessToken : 추가적인 Naver API 사용 시 필요
      async (accessToken, refreshToken, profile, done) => {
        try {
          let user = await authStorage.getOauthUserById(profile.id);

          if (!user) {
            // 사용자 없으면 새로 생성
            user = {
              provider: profile.provider,
              provider_user_id: profile.id,
              nickname: profile.name,
              email: profile.email,
              accessToken,
              refreshToken,
            };

            await authStorage.createOauthUser(user);
          } else {
            // 사용자 있으면 Update
            await authStorage.UpdateOauthUser({
              accessToken: accessToken || user.accessToken,
              refreshToken: refreshToken || user.refreshToken,
              provider_user_id: profile.id,
            });
          }

          // 인증 실패
          return done(null, user);
        } catch (err) {
          return done(err);
        }
      }
    )
  );
};
```

---

## router 작성

routes 폴더에 `auth.route.js` 파일을 생성합니다.

```js
import express from "express";
import ctrl from "../controllers/auth.controller.js";

const router = express.Router();

// 네이버 OAuth
router.get("/naver", ctrl.naver);
router.get("/naver/callback", ctrl.naverCallback);

export default router;
```

---

## controller 작성

controllers 폴더에 `auth.controller.js` 파일을 생성합니다.

```js
import passport from "passport";
import tokenUtil from "../utils/token.util.js";

export default {
  naver: (req, res, next) => {
    const redirectUrl = req.query.redirectUrl || "http://localhost:3000"; // 기본값 설정

    // 네이버 로그인 페이지로 리디렉션
    passport.authenticate("naver", {
      session: false,
      state: encodeURIComponent(redirectUrl),
    })(req, res, next);
  },

  naverCallback: (req, res, next) => {
    try {
      // 로그인 콜백
      passport.authenticate(
        "naver",
        {
          session: false, // 세션을 사용하지 않음 (JWT 사용 시)
          failureRedirect: "/", // 실패 시 이동할 URL
        },
        async (err, user) => {
          const redirectUrl = req.query.state
            ? decodeURIComponent(req.query.state)
            : "http://localhost:3000/movie";

          if (err || !user) {
            return res.redirect(redirectUrl);
            // 로그인 실패 시 리디렉션
          }

          // AccessToken 생성
          const accessToken = await tokenUtil.generateAccessToken({
            id: user.provider_user_id,
            provider: user.provider,
          });

          // RefreshToken 생성
          const refreshToken = await tokenUtil.generateRefreshToken({
            id: user.provider_user_id,
            provider: user.provider,
          });

          res.cookie("accessToken", accessToken, {
            sameSite: "strict",
            httpOnly: true,
            secure: process.env.NODE_ENV === "production", // https Only
          });

          // RefreshToken 쿠키로 발급
          res.cookie("refreshToken", refreshToken, {
            sameSite: "strict",
            httpOnly: true,
            secure: process.env.NODE_ENV === "production",
          });

          // 클라이언트로 리디렉션
          res.redirect(redirectUrl);
        }
      )(req, res, next);
    } catch (err) {
      next(err); // 예외 처리
    }
  },
};
```
