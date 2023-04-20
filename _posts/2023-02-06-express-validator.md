---
title: express-validator 모듈
author: Psmin
data: 2023-02-06 22:41:12 +0900
categories: [Nodejs]
tags: [express-validator]
---

# express-validator 모듈에 대해 알아보자.

---

## 유효성 검사

유효성 검사는 데이터가 서버나 데이터베이스 보내지기 전에 개발자가 만든 조건에 부합하는지 확인, 검증하는 작업을 말합니다.

간단한 예로 가입 절차를 진행하다가 **_"이미 가입된 아이디입니다."_**, **_"비밀번호는 영문,숫자,특수문자가 혼합되어야 합니다"_** 등 사용자는 개발자가 원하는 조건에 맞게 데이터를 입력하도록 유도합니다.

- `Form Validation`  
  : Client 측에서 일반적으로 많이 사용하는 유효성 검사입니다.  
  <kbd>input</kbd> 태그에 <kbd>attributes</kbd> 를 설정하는 것으로 <kbd>submit</kbd> 실행 전 유효성 검사르 진행합니다.

클라이언트 측에서의 검사가 속도면에서는 빠르지만 데이터의 중복 등 데이터베이스에 의존하는 작업들은 서버에서 검사하는 것이 좋습니다.

---

## express-validator 모듈

express-validator 모듈은 express에서 주로 사용하는 유효성 검사 모듈입니다.

직관적이고, 어떤 부분의 유효성을 검사하는지 알 수 있는 장점이 있습니다.

- 설치
  ```console
  npm i express-validator
  ```

---

## 유효성 검사 체인 (Validation Chain)

먼저 하나 이상의 필드에 Validation Chain을 생성합니다.

필드는 다음 요청 위치중 하나에서 선택됩니다.

- `req.body`
- `req.cookies`
- `req.headers`
- `req.query`
- `req.params`

- 생성 메서드
  : method('필드', '메세지') 형태로 Validation Chain을 생성합니다.

  - `check()`  
    : express-validator로 HTTP 요청을 검증하고 삭제하는 데 사용되는 기본 API입니다.

  - `body()`  
    : `check()`와 동일하지만 <kbd>req.body</kbd>만 확인합니다.

  - `cookies()`  
    : `check()`와 동일하지만 <kbd>req.cookies</kbd>만 확인합니다.
  - `headers()`  
    : `check()`와 동일하지만 <kbd>req.headers</kbd>만 확인합니다.
  - `params()`  
    : `check()`와 동일하지만 <kbd>req.params</kbd>만 확인합니다.
  - `query()`  
    : `check()`와 동일하지만 <kbd>req.query</kbd>만 확인합니다.

{: .prompt-info}

> Validator Chain에 포함된 validators, sanitizers, Modifiers 메서드로 유효성 검사 동작을 조정합니다.

---

## Built-in validators

내장 메서드 중 주로 실질적인 유효성 검사를 진행합니다.

- `custom()`
  : 사용자 지정 Validator 함수를 추가합니다.

  - **_예시_**  
    다음 예시는 회원가입을 할 때 email 아이디가 이미 존재하는 지 검사합니다.
    ```js
    app.post('/signup', body('email').custom(async value => {
      const existingUser = await Users.findUserByEmail(value); // email 값으로 사용자 정보 조회 부분
      if (existingUser) { // 같은 이메일의 사용자가 있다면
        throw new Error('E-mail already in use'); // 에러 던지기
      }
    })), (req, res) => {
      // Handle request
    });
    ```

- `exists()`  
  : 필드가 존재하는지 확인하는 유효성 검사를 추가합니다.

  ```js
  exists(options?: {
    values?: 'undefined' | 'null' | 'falsy',
  })
  ```

  - <kbd>options.value</kbd>로 존재의 기준을 정합니다.
    - **_undefined_** : undefined 값을 기준으로 존재 확인
    - **_null_** : undefined, null 값을 기준으로 존재 확인
    - **_falsy_** : undefined, null, false, 잘못된 값(빈 문자열, 0 등) 값을 기준으로 존재 확인

- `isArray()`  
  : 필드가 존재하는지 확인하는 유효성 검사를 추가합니다.

  ```js
  isArray(options?: { min?: number; max?: number })
  ```

  - <kbd>options.min</kbd>, <kbd>options.max</kbd> 값은 배열의 길이를 검사합니다.

- `isObject()`  
  : 값이 객체인지 확인하는 유효성 검사를 추가합니다.

  ```js
  isObject(options?: { strict?: boolean })
  ```

  - <kbd>options.strict</kbd> 값을 `false`로 주면 순수 자바스크립트에서의 `typeof value ==='object'` 와 유사하게 동작합니다.

- `isStriong()`  
  : 값이 문자열인지 확인하는 유효성 검사를 추가합니다.

- `notEmpty()`  
  : 값이 비어 있지 않은 문자열인지 확인하는 유효성 검사를 추가합니다.

- 그 외 주요 메서드

  - `isLength()`  
    : 길이가 num인지 확인해줍니다.  
    { min: num, max: num } 과 같이 최소, 최대 값도 지정할 수 있습니다.

  - `isNumeric()`  
    : 숫자 형태인지 확인합니다.  
    string이어도 해당 string이 숫자인지 확인해줍니다.

  - `isEmail()`  
    : string이 이메일 형태인지 확인해줍니다.

  - 내장 메서드 확인 링크 - **Github 페이지**(<https://github.com/validatorjs/validator.js>)

---

## Built-in Sanitizer

내장 메서드 중 주로 살균, 보안 위협을 제거하기 위한 검사를 진행합니다.

- `customSanitizer()`  
  : 체인에 사용자 지정 Sanitizer 함수를 추가합니다.

- `default()`  
  : 필드 값이 <kbd>null</kbd>, <kbd>undefined</kbd>, <kbd>NaN</kbd>인 경우 기본 값으로 바꿔줍니다.

  ```js
  app.post("/", body("username").default("foo"), (req, res, next) => {
    // 'bar'     => 'bar'
    // ''        => 'foo'
    // undefined => 'foo'
    // null      => 'foo'
    // NaN       => 'foo'
  });
  ```

- `replace()`
  : 필드 값을 확인하고 바꿉니다.

  ```js
  app.post(
    "/",
    body("username").replace(["bar", "BAR"], "foo"),
    (req, res, next) => {
      // 'bar_' => 'bar_'
      // 'bar'  => 'foo'
      // 'BAR'  => 'foo'
    }
  );
  ```

- `toArray()`  
  : 값을 배열로 변환합니다.

- `toLowerCase()`
  : 값을 소문자로 변환합니다.

- `toUpperCase()`
  : 값을 대문자로 변환합니다.

- `trim()`
  : 양쪽 공백을 제거합니다.

---

## Modifiers

- `bail()`  
  : 이전 유효성 검사 중 하나라도 실패하면 유효성 검사 체인 실행을 중지합니다.  
  또한, `bail()`은 여러 번 사용 가능합니다.

  ```js
  body("username")
    .isEmail()
    // If not an email, stop here
    .bail()
    .custom(checkDenylistDomain)
    // If domain is not allowed, don't go check if it already exists
    .bail()
    .custom(checkEmailExists);
  ```

- `if()`
  : 유효성 검사 체인에 조건문을 추가합니다.

  ```js
  body("newPassword")
    .if(body("oldPassword").notEmpty())
    // The length of the new password will only be checked if `oldPassword` is provided.
    .isLength({ min: 6 });
  ```

- `withMessage()`
  : 오류 메시지를 설정합니다.

---

## Validation Results

- `validationResult()`  
  : 유효성 검사 결과를 추출하고 Result 객체에 반환합니다.

  ```js
  const { query, validationResult } = require("express-validator");

  app.post("/hello", query("person").notEmpty(), (req, res) => {
    const result = validationResult(req);
    // Use `result` to figure out if the request is valid or not
  });
  ```

---

# 참조

- <https://express-validator.github.io/docs/api/validation-chain#standard-validators>
