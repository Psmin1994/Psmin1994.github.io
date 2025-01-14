---
title: Server - 08
author: Psmin
data: 2024-06-15 19:23:21 +0900
categories: [Project, Movie Story]
tags: [passport, jwt]
---

---

# 로그인 상태 유지

인증을 구현했으니 로그인 상태를 유지해보겠습니다.

> 참고글 : [jwt](https://psmin1994.github.io/posts/jwt/), [로그인 페이지 만들기 - 09](https://psmin1994.github.io/posts/login-09/)

---

## jwt 토큰 방식

서버는 로그인 성공 시, 토큰을 생성해 전달합니다.

사용자는 이후의 서버로 요청을 보낼 때, 토큰을 함께 보내는 것으로 인증 절차를 대신할 수 있습니다.

> AccessToken, RefreshToken을 사용합니다. (자세한 내용은 참고글을 확인하세요.)

---

## token.util.js 작성

- `sign` 메서드를 사용해 토큰 생성 함수 구현

- `verify` 메서드를 사용해 토큰 검증 함수를 구현

- RefreshToken은 서버 측에서 따로 저장 (Redis 사용)

```js
import jwt from "jsonwebtoken";
import redisClient from "../config/redis.js";

const createTokenError = (message, code) => {
  const error = new Error(message);
  error.code = code;
  throw error;
};

export default {
  // AccessToken 생성
  generateAccessToken: (payload) => {
    try {
      return jwt.sign(payload, process.env.ACCESS_TOKEN_SECRET, {
        expiresIn: "30m", // 유효기간
        algorithm: "HS256", // 암호화 알고리즘
      });
    } catch (err) {
      createTokenError("Generate access token fail", "ACCESS_TOKEN_ERROR");
    }
  },

  // RefreshToken 생성 + Redis에 저장
  generateRefreshToken: async (payload) => {
    try {
      const refreshToken = jwt.sign(payload, process.env.REFRESH_TOKEN_SECRET, {
        expiresIn: "3h",
        algorithm: "HS256",
      });

      // RefreshToken Redis에 저장
      redisClient.set(payload.id, refreshToken, { EX: 60 * 60 * 3 });

      return refreshToken;
    } catch (err) {
      createTokenError("Generate refresh token fail", "REFRESH_TOKEN_ERROR");
    }
  },

  // AccessToken 검증
  verifyAccessToken: (accessToken) => {
    try {
      return jwt.verify(accessToken, process.env.ACCESS_TOKEN_SECRET);
    } catch (err) {
      if (err.name === "TokenExpiredError") {
        createTokenError("Refresh token expired", "TOKEN_EXPIRED");
      } else {
        throw err; // 기타 예상치 못한 오류
      }
    }
  },

  // RefreshToken 검증
  verifyRefreshToken: async (refreshToken) => {
    try {
      const decode = jwt.verify(refreshToken, process.env.REFRESH_TOKEN_SECRET);

      // Redis에 저장된 RefreshToken 가져오기
      const storedToken = await redisClient.get(decode.id);

      if (!storedToken) {
        createTokenError("No refreshToken in Redis", "TOKEN_NOT_FOUND");
      }

      return decode;
    } catch (err) {
      if (err.name === "TokenExpiredError") {
        createTokenError("Refresh token expired", "TOKEN_EXPIRED");
      } else if (err.name === "JsonWebTokenError") {
        createTokenError("No refresh token", "TOKEN_NOT_FOUND");
      } else {
        // 기타 예상치 못한 오류
        throw err;
      }
    }
  },

  DeleteRefreshToken: async (refreshToken) => {
    try {
      const decode = jwt.verify(refreshToken, process.env.REFRESH_TOKEN_SECRET);

      const deleteResult = await redisClient.del(`${decode.id}`);

      return deleteResult;
    } catch (err) {
      throw err; // 기타 예상치 못한 오류
    }
  },
};
```

---

## Controller 파일 수정

`user.controller.js` 파일에서 login 인증 성공 시, Token을 생성하고 Client에 전달합니다. (Cookie 사용)

또한, 사용자가 Logout 요청 시, Token을 삭제합니다.

```js
import passport from "passport";
import userStorage from "../models/user.model.js";
import { createPasswordAndSalt } from "../utils/crypto.util.js";

export default {
  login: (req, res, next) => {
    passport.authenticate("local", async (err, user, response) => {
      if (err) return next(err);

      if (!user) return res.json(response);

      / AccessToken 생성
        const accessToken = await tokenUtil.generateAccessToken({ id: userId });

        // RefreshToken 생성
        const refreshToken = await tokenUtil.generateRefreshToken({ id: userId });

        // RefreshToken 쿠키로 발급
        res.cookie("accessToken", accessToken, {
          sameSite: "strict",
          httpOnly: true,
          secure: false, // https Only
        });

        // RefreshToken 쿠키로 발급
        res.cookie("refreshToken", refreshToken, {
          sameSite: "strict",
          httpOnly: true,
          secure: false,
        });

      res.status(200).json({
        success: true,
        msg: "로그인 성공",
      });
    })(req, res, next); // 미들웨어 내의 미드뤠어는 (req, res, next)를 붙여줍니다.
  },

  register: async (req, res, next) => {
    ...
  },

  // 로그아웃 기능 구현
  logout: async (req, res, next) => {
    try {
      // 로그아웃 요청시 토큰을 쿠키에서 삭제
      res.clearCookie("accessToken", {
        sameSite: "strict",
        httpOnly: true,
        secure: false,
      });

      res.clearCookie("refreshToken", {
        sameSite: "strict",
        httpOnly: true,
        secure: false,
      });

      // Redis 안에 저장되어 있는 RefreshToken 삭제
      if (req.cookies.refreshToken) await tokenUtil.DeleteRefreshToken(req.cookies.refreshToken);

      return res.json({ message: "User Logout" });
    } catch (err) {
      next(err);
    }
  },
}
```

사용자를 로그인 성공 시, Token을 받습니다.

Token은 서버에 요청을 보낼 때, 서버에서 확인 후 로그인 인증 과정 없이 서비스를 제공해주는 역할을 합니다.

---

## token.verify.js 작성

서비스 이용 전 Token을 검증하는 미들웨어로 Router 단계에서 Controller로 넘어가기전 거쳐갑니다.

passport-jwt 모듈을 활용해보겠습니다.

```js
import passport from "passport";

const tokenVerity = (req, res, next) => {
  passport.authenticate(
    "jwt",
    { session: false },
    async (err, user, response) => {
      if (err) {
        return next(err);
      }

      if (!user) {
        if (response && response.message === "No auth token") {
          return res.status(401).json({ message: "No access token" });
        }

        if (response && response.message === "jwt expired") {
          return res.status(401).json({ message: "Access token expired" });
        }
      }

      req.user = user;

      next();
    }
  )(req, res, next);
};

export default tokenVerity;
```

---

## jwtStrategy.js 작성

passport 폴더의 `index.js` 파일에 추가할 jwt 전략 파일을 작성합니다.

```js
import { Strategy as JwtStrategy } from "passport-jwt";
import userStorage from "../models/user.model.js";

const cookieExtractor = (req) => {
  let token = null;

  if (req && req.cookies) {
    token = req.cookies.accessToken;
  }

  return token;
};

export default (passport) => {
  passport.use(
    "jwt",
    new JwtStrategy(
      {
        jwtFromRequest: cookieExtractor,
        secretOrKey: process.env.ACCESS_TOKEN_SECRET, // 암호 해독 키
        ignoreExpiration: false, // 만료된 토큰은 거부
      },
      async (payload, done) => {
        try {
          let id = payload.id;

          let user = await userStorage.getUserInfo(id);

          if (user) {
            return done(null, user); // 사용자 있으면 인증 성공
          }

          return done(null, false); // 사용자 없으면 인증 실패
        } catch (err) {
          done(err);
        }
      }
    )
  );
};
```

작성한 passport 전략을 `index.js`에 추가합니다.

```js
import passport from "passport";
import local from "./localStrategy.js";
import jwt from "./jwtStrategy.js";

// 로컬 로그인
local(passport);

// JWT 검증
jwt(passport);

export default passport;
```

---

## 미들웨어 적용

사용자 인증이 필요한 서비스에 `tokenVerity 미들웨어`를 추가합니다.

```js
import express from "express";
import ctrl from "../controllers/user.controller.js";
import userValidator from "../middleware/user.validation.js";

// Token 검증 미들웨어
import tokenVerity from "../middleware/token.verify.js";

const router = express.Router();

router.get("/info", tokenVerity, ctrl.info); //
router.get("/logout", tokenVerity, ctrl.logout); // 유저 로그아웃

router.post("/login", ctrl.login); // 유저 로그인
router.post("/register", userValidator.register, ctrl.register); // 유저 회원가입

// AccessToken 만료 시, Token 재발급
router.get("/refresh", ctrl.refreshToken);

export default router;
```

---

## Token 재발급

`user.controller.js`에 RefreshToken을 검증하고 AccessToken을 재발급하는 과정을 구현합니다.

```js
import passport from "passport";
import userStorage from "../models/user.model.js";
import tokenUtil from "../utils/token.util.js";

export default {

  ...

  refreshToken: async (req, res, next) => {
    try {
      const refreshToken = req.cookies.refreshToken;

      // RefreshToken 검증
      const refreshDecode = await tokenUtil.verifyRefreshToken(refreshToken);

      let user = null;
      let newAccessToken = null;

      // RefreshToken은 유효인 경우
      user = await userStorage.getUserById(refreshDecode.id);

      // AccessToken 재발급
      newAccessToken = await tokenUtil.generateAccessToken({ id: user.id });

      res.cookie("accessToken", newAccessToken, {
        sameSite: "strict",
        httpOnly: true,
        secure: false,
      });

      return res.json({ message: "Token refreshed" });
    } catch (err) {
      if (err.code === "TOKEN_EXPIRED") {
        // RefreshToken 만료인 경우
        res.clearCookie("accessToken", {
          sameSite: "strict",
          httpOnly: true,
          secure: false,
        });

        res.clearCookie("refreshToken", {
          sameSite: "strict",
          httpOnly: true,
          secure: false,
        });

        return res.status(401).json({ message: err.message });
      }

      if (err.code === "TOKEN_NOT_FOUND") {
        // AccessToken이 없는 경우
        res.clearCookie("accessToken", {
          sameSite: "strict",
          httpOnly: true,
          secure: false,
        });

        return res.status(401).json({ message: err.message });
      }

      if (err.code === "ACCESS_TOKEN_ERROR") {
        return res.status(401).json({ message: err.message });
      }

      next(err);
    }
  },

  ...

};
```
