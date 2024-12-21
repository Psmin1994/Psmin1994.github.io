---
title: Server - 06
author: Psmin
data: 2024-02-22 17:32:51 +0900
categories: [Project, Movie Story]
tags: [crypto]
---

# 비밀번호 암호화

> 참고 글 : [crypto](https://psmin1994.github.io/posts/crypto/),[로그인 페이지 만들기 - 07](https://psmin1994.github.io/posts/login-07/)

---

## 비밀번호 암호화 함수

32 Byte의 임의의 문자열을 salt로 만들고 crypto의 `pbkdf2Sync` 메서드를 사용해 암호화합니다.

```js
// 암호화된 비밀번호 생성 함수
const createPasswordAndSalt = (userInputPassword) => {
  try {
    const salt = crypto.randomBytes(32).toString("base64");

    const key = crypto
      .pbkdf2Sync(userInputPassword, salt, 200, 8, "sha512")
      .toString("base64");

    return `${key}$${salt}`;
    // 암호화된 비밀번호와 임의로 생성된 salt를 '$'로 구분합니다.
  } catch (err) {
    throw err;
  }
};
```

---

## 비밀번호 검증 함수

```js
// 비밀번호 검증 함수
// 사용자의 입력 값과 데이터베이스에 저장된 암호화된 값을 비교해 검증합니다.
const userPasswordVerify = (userInputPassword, storedPasswordAndSalt) => {
  try {
    const [encrypted, salt] = storedPasswordAndSalt.split("$");

    const userInputEncrypted = crypto
      .pbkdf2Sync(userInputPassword, salt, 200, 8, "sha512")
      .toString("base64");

    if (userInputEncrypted === encrypted) {
      return true;
    } else {
      return false;
    }
  } catch (err) {
    throw err;
  }
};
```

---

## crypto.util.js

`src`폴더 안에 `utils`폴더를 만들고 `crypto.util.js`파일을 작성합니다.

```js
import crypto from "crypto";

// 암호화된 비밀번호 생성 함수
const createPasswordAndSalt = (userInputPassword) => {
  try {
    const salt = crypto.randomBytes(32).toString("base64");

    // 매개변수 : 입력값, salt, 반복 횟수, Key의 길이, 해시알고리즘
    const key = crypto
      .pbkdf2Sync(userInputPassword, salt, 200, 8, "sha512")
      .toString("base64");

    return `${key}$${salt}`;
    // 암호화된 비밀번호와 임의로 생성된 salt를 '$'로 구분
  } catch (err) {
    throw err;
  }
};

// 비밀번호 검증 함수
// 사용자의 입력 값과 데이터베이스에 저장된 암호화된 값을 비교해 검증합니다.
const userPasswordVerify = (userInputPassword, storedPasswordAndSalt) => {
  try {
    const [encrypted, salt] = storedPasswordAndSalt.split("$");

    const userInputEncrypted = crypto
      .pbkdf2Sync(userInputPassword, salt, 200, 8, "sha512")
      .toString("base64");

    if (userInputEncrypted === encrypted) {
      return true;
    } else {
      return false;
    }
  } catch (err) {
    throw err;
  }
};

export { createPasswordAndSalt, userPasswordVerify };
```

---

## 암호화 적용

`user.controller.js` 파일을 수정합니다.

1. 회원가입 시, 암호화된 비밀번호를 DB에 저장합니다.

2. 로그인 시, DB에서 가져온 암호화된 비밀번호를 검증 함수를 사용해 입력 값과 비교합니다.

```js
import userStorage from "../models/user.model.js";
import {
  createPasswordAndSalt,
  userPasswordVerify,
} from "../utils/crypto.util.js";

export default {
  login: async (req, res, next) => {
    let response = {};

    try {
      let { id, password } = req.body;

      const user = await userStorage.getUserInfo(id);

      if (!user) {
        response = {
          success: false,
          msg: "아이디가 존재하지 않습니다",
        };
      } else {
        // 비밀번호 검증
        let checkedPassword = await userPasswordVerify(password, user.password);

        if (checkedPassword) {
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
        // 암호화된 비밀번호 생성
        req.body.password = await createPasswordAndSalt(password);

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
