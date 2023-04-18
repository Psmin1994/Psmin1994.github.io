---
title: express-validator 모듈이란?
author: Psmin
data: 2023-02-08 22:41:12 +0900
categories: [Nodejs]
tags: [express-validator]
---

# express-validator 모듈에 대해 알아보자.

---

## 유효성 검사

유효성 검사는 데이터가 서버나 데이터베이스 보내지기 전에 개발자가 만든 조건에 부합하는지 확인, 검증하는 작업을 말합니다.

간단한 예로 가입 절차를 진행하다가 **_"이미 가입된 아이디입니다."_**, **_"비밀번호는 영문,숫자,특수문자가 혼합되어야 합니다"_** 등 사용자는 개발자가 원하는 조건에 맞게 데이터를 입력하도록 유도합니다.

- <kbd>Form Validation</kbd>  
  Client 측에서 일반적으로 많이 사용하는 유효성 검사입니다.  
  <kbd>input</kbd> 태그에 <kbd>attributes</kbd> 를 설정하는 것으로 <kbd>submit</kbd> 실행 전 유효성 검사르 진행합니다.

클라이언트 측에서의 검사가 속도면에서는 빠르지만 데이터의 중복 등 데이터베이스에 의존하는 작업들은 서버에서 검사하는 것이 좋습니다.

---

## express-validator 모듈

express-validator 모듈은 express에서 주로 사용하는 유효성 검사 모듈입니다.

- 설치
  ```console
  npm i express-validator
  ```
