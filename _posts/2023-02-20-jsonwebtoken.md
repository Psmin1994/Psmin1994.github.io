---
title: jsonwebtoken 모듈
author: Psmin
data: 2023-02-20 21:23:16 +0900
categories: [Knowledge, Nodejs]
tags: [jsonwebtoken]
---

# jsonwebtoken 모듈에 대해 알아보자.

---

## jsonwebtoken 모듈

**jsonwebtoken** 모듈은 JWT를 쉽게 사용하도록 지원하는 패키지입니다.

**jsonwebtoken** 모듈은 함수이므로 따로 미들웨어로 등록하지 않고 import해 바로 사용할 수 있습니다.

주로, 토큰 암호화 및 발급 인증 등을 지원합니다.

- 설치
  ```js
  npm i jsonwebtoken
  ```

---

## 토큰 생성 메서드 - sign

토큰 생성 메서드로 **sign** 메서드를 사용합니다.

- `jwt.sign(payload, secretkey, [options, callback])` 형태

  - `payload`  
    : 객체 리터럴, 버퍼, JSON 등으로 토큰에 담고 싶은 정보입니다.

  - `secretkey`  
    : 일반 스트링, 버퍼, 객체 등으로 암호화키입니다.

  - `options`  
    : 추가 옵션으로 객체로 입력합니다. 주로, 사용할 암호화 알고리즘이나 만료시간 등을 설정합니다.

    - **expiresIn** : 만료 시각
    - **issuer** : 토큰 발급자
    - **subject** : 토큰 제목

  - `callback`  
    : 콜백 추가시 비동기적으로 수행되며 에러와 토큰을 반환, 생략 시 토큰만 반환됩니다.

---

## 토큰 검증 메서드 - verify

토큰 검증 메서드로 **verify** 메서드를 사용합니다.

여기서 검증은 서버에서 발급한 토큰이 맞는 지 확인하는 것을 말합니다.

- `jwt.verify(token, secretkey, [options, callback])` 형태

  - `token`  
    : 검증 할 토큰을 입력합니다.

  - `secretkey`  
    : 해당 토큰을 생성할 때 사용한 secretkey를 입력합니다.

  - `options`  
    : 추가 옵션으로 객체 형태로 입력합니다.

    - **iat** : 토큰이 발급된 시간 (issued at)
    - **exp** : 만료 시각 (expiraton)
    - **iss** : 토큰 발급자 (issuer)
    - **sub** : 토큰 제목 (subject)
    - **aud** : 토큰 대상자 (audience)

  - `callback`  
    : 콜백 추가시 비동기적으로 수행되며 에러와 결과를 반환합니다.

---

## 간단 예제

**sign** 메서드에 첫 번째 인자로 주어진 정보가 **verify** 메서드로 확인시 잘 나오는지 확인해보자.

1. **토큰 생성 해보기**

   ```js
   import jwt from "jsonwebtoken";

   var token = jwt.sign(
     {
       test: "test",
     },
     "secretkey",
     {
       // algorithm,
       subject: "test jwttoken",
       expiresIn: "60s", // 만료 시간 지정
       issuer: "psmin", //
     }
   );

   console.log(token);
   ```

   ![jwt-ex-01](/assets/img/jwt-ex-01.png){: .normal}

2. 생성한 토큰 검증해보기

   ```js
   import jwt from "jsonwebtoken";

   var token = {
     ...
   }

   var check = jwt.verify(token, "secretkey", (err, result) => {
     if (err) return err;

     return result;
   });

   console.log(check);

   ```

   ![jwt-ex-02](/assets/img/jwt-ex-02.png){: .normal}

---

## 참조

- <https://www.npmjs.com/package/jsonwebtoken>
- <https://wky.kr/39>
