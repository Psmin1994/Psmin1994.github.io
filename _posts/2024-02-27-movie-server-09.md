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

### naverStrategy.js

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

## oauth_user 테이블 설계

`console.log(profile)`로 출력된 값을 살펴보겠습니다.

```
{
  provider: 'naver',
  id: 'cPJ5IXic2cC-OlxKWxoJOAOfxWFETlFxnLWEI8r_yiE',
  nickname: '박상민',
  profileImage: undefined,
  age: undefined,
  gender: undefined,
  email: 'tkdals6405@naver.com',
  mobile: undefined,
  mobileE164: undefined,
  name: '박상민',
  birthday: undefined,
  birthYear: undefined,
  _raw: '{"resultcode":"00","message":"success","response":{"id":"cPJ5IXic2cC-OlxKWxoJOAOfxWFETlFxnLWEI8r_yiE","nickname":"\\ubc15\\uc0c1\\ubbfc","email":"tkdals6405@naver.com","name":"\\ubc15\\uc0c1\\ubbfc"}}',
  _json: {
    resultcode: '00',
    message: 'success',
    response: {
      id: 'cPJ5IXic2cC-OlxKWxoJOAOfxWFETlFxnLWEI8r_yiE',
      nickname: '박상민',
      email: 'tkdals6405@naver.com',
      name: '박상민'
    }
  }
}
```

- **provider** :
