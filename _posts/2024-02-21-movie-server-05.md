---
title: 영화 정보 제공 사이트 (Server) - 05
author: Psmin
data: 2024-02-21 20:11:54 +0900
categories: [Project, Movie Story]
tags: [validation]
---

# 유저 회원가입 Validation 구현

> 참고 글 : [express-validator](https://psmin1994.github.io/posts/express-validator/),[로그인 페이지 만들기 - 06](https://psmin1994.github.io/posts/login-06/)

---

## validation 구현

먼저 회원 정보 관련 요구사항을 추가 정리합니다.

1. 아이디는 4 ~ 12 글자이어야 하고 영어 대소문자, 숫자로만 구성되어야합니다.

2. 비밀번호는 4 ~ 12 글자이어야 하고 영어, 숫자, 특수문자(!@#$%^&\*)가 하나 이상 포함되어야합니다.

---

### user.validation.js

`middleware`폴더에 `user.validation.js`파일을 만들어서 사용하겠습니다.

> error 발생 시, 메세지를 담은 객체를 반환합니다.

```js
import { body, validationResult } from "express-validator";

const validator = (req, res, next) => {
  // validationResult 메서드를 통해 결과를 받습니다.
  let errors = validationResult(req);

  // isEmpty메소드는 express-validator 내장 메소드
  if (!errors.isEmpty()) {
    const response = {};

    // array메소드는 express-validator 내장 메소드
    for (let element of errors.array()) {
      response[element.path] = element.msg;
    }

    return res.status(400).json(response);
  }

  next();
};

const userValidator = {
  register: [
    body("id")
      .trim() // 양쪽 공백 제거
      .notEmpty() // 공백이 아닌지
      .withMessage("아이디를 입력해주세요") // 메세지 생성
      .bail() // 메세지 생성 시 종료
      .isLength({ min: 4, max: 12 }) // 길이 확인 메서드
      .withMessage("아이디는 4글자 이상 12글자 미만이어야합니다")
      .bail()
      .isAlphanumeric() // 영어 대소문자, 숫자 구조 확인 메서드
      .withMessage("아이디는 숫자, 영어 대소문자로 구성해주세요")
      .bail(),

    body("password")
      .notEmpty()
      .withMessage("비밀번호를 입력하세요")
      .bail()
      .isLength({ min: 4, max: 12 })
      .withMessage("비밀번호는 4글자 이상 12글자 미만이어야합니다")
      .bail()
      .matches(/[a-zA-Z]/)
      .withMessage("비밀번호는 영어를 최소 1자 이상 포함해야 합니다")
      .bail()
      .matches(/\d/)
      .withMessage("비밀번호는 숫자를 최소 1자 이상 포함해야 합니다")
      .bail()
      .matches(/[!@#$%^&*]/)
      .withMessage(
        "비밀번호는 특수문자(!@#$%^&*)를 최소 1자 이상 포함해야 합니다"
      )
      .bail(),
    ,
    body("confirmPassword")
      .notEmpty()
      .withMessage("비밀번호를 한번 더 입력해주세요")
      .bail()
      .custom((value, { req }) => {
        return value === req.body.password;
      })
      .withMessage("입력한 비밀번호와 같지 않습니다.")
      .bail(),

    body("name")
      .notEmpty()
      .withMessage("닉네임을 입력해주세요")
      .bail()
      .isLength({ min: 3, max: 10 })
      .withMessage("닉네임은 3글자 이상 10글자 미만이어야합니다")
      .bail()
      .custom((value) => {
        // 한글, 영어 대소문자, 숫자
        let checkRegExp = /^[가-힣A-Za-z0-9]+$/;

        return checkRegExp.test(value);
      })
      .withMessage("아이디는 한글, 숫자, 영어 대소문자로 구성해주세요")
      .bail(),

    validator,
  ],
};

export default userValidator;
```

---

### router

routes 폴더에 `user.validation.js` import해서 사용합니다.

controller로 넘어가기전 유효성 검사를 진행합니다.

```js
import express from "express";
import ctrl from "../controllers/user.controller.js";
import userValidator from "../middleware/user.validation.js";

const router = express.Router();

router.post("/login", ctrl.login); // 유저 로그인
router.post("/register", userValidator.register, ctrl.register); // 유저 회원가입

export default router;
```
