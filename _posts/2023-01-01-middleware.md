---
title: 미들웨어 Middleware
author: Psmin
data: 2023-01-01 14:29:13 +0900
categories: [Knowledge, NodeJS]
tags: [Express, Middleware]
---

## 미들웨어 Middleware

미들웨어는 Express 동작의 핵심으로 HTTP 요청과 응답 사이에서 단계별 동작을 수행합니다.

**docs**를 보면 middleware에 대한 설명이 나와있습니다.

> "Middleware functions are functions that have access to the request object (req), the response object (res), and the next function in the application’s request-response cycle."

번역하면 **_"미들웨어 함수는 req(요청) 객체, res(응답) 객체, 그리고 어플리케이션 요청-응답 사이클 도중 그 다음의 미들웨어 함수에 대한 엑세스 권한을 갖는 함수이다."_** 입니다.

간단하게 말하면 클라이언트에게 요청이 오면 그 요청과 응답 중간에서 목적에 맞게 처리를 하는, 말하자면 거쳐가는 함수들이라고 볼 수 있습니다.

![Middleware](/assets/img/middleware.jpg){: .normal}

---

## 미들웨어 작성법

```js
app.get('/', (req, res) => {
  ...
}
```

기본적으로 req, res, next를 가진 함수를 작성하면 해당 함수는 미들웨어로 동작할 수 있습니다.

- `req` : HTTP 요청을 처리하는 객체
- `res` : HTTP 응답을 처리하는 객체
- `next` : 다음 미들웨어를 실행하는 함수

---

### 미들웨어 작성 예제

Express 에서 미들웨어를 작성 기본 예제를 살펴보겠습니다.

```js
var express = require("express");

var app = express();

// app.use()의 인자안의 화살표 함수가 미들웨어
app.use((req, res, next) => {
  req.requestTime = Date.now(); // req에 requestTime 키와 밸류를 등록합니다.
  next(); // 다음 미들웨어 함수를 작동
});

// app.get()의 인자안의 화살표 함수가 미들웨어
app.get("/", (req, res) => {
  // 위에서 next()가 호출되면서 작동
  res.send(req.requestTime);
  // 위 미들웨어에서 requestTime 객체를 등록했고 next()를 사용했기 때문에 호출해서 데이터 사용 가능
});

app.listen(3000);
```

`use`, `method(get, post, put, delete 등)` 메서드로 미들웨어를 연결합니다.

`next()` 콜백 함수의 호출로 다음 미들웨어를 실행할 수 있습니다.

---

### 애플리케이션 미들웨어

미들웨어는 적용되는 위치에 따라서 분류합니다.

애플리케이션 미들웨어는 **모든 요청에 공통적으로 적용**하기위한 방법입니다.

http 요청이 들어온 순간부터 적용된 순서대로 동작합니다.

```js
app.use((req, res, next) => {
  console.log("모든 요청에 다 실행됩니다.");

  next();
});
```

---

### 라우터 미들웨어

특정 경로의 라우팅에만 미들웨어를 적용하는 방법입니다.

```js
// /error 요청 올때 동작
app.get("/error", (req, res, next) => {
  console.log("error 발생");
  next();
});

// /about 요청 올때 동작
app.get("/about", (req, res) => {
  res.send("Hello, about");
});
```

---

### 오류처리 미들웨어

일반적으로 마지막에 위치하는 미들웨어로 요청에 따라 미들웨어를 순차적으로 처리하다가 앞선 미들웨어에서 next 함수에 인자가 전달되면 바로 넘어오는 미들웨어입니다.

반드시 err, req, res, next 4개의 인자를 갖습니다.

```js
app.use((req, res, next) => {
  if (!isAdmin(req)) {
    // 중간은 건너뛰고 마지막 오류처리 미들웨어로 넘어갑니다.
    next(new Error("Not Authorized"));
    return;
  }
  next();
});

app.get("/", (req, res, next) => {
  res.send("Hello Express");
});

// 오류 처리 미들웨어
app.use((err, req, res, next) => {
  res.send("Error Occurred");
});
```

가장 아래 적용된 err, req, res, next를 인자로 갖는 함수가 오류처리 미들웨어입니다.

이전에 적용된 미들웨어 중 next에 인자를 넘기는 경우 중간 미들웨어들은 뛰어넘고 오류처리 미들웨어가 바로 실행됩니다.

---

### 미들웨어 서브스택

여러 개의 미들웨어를 동시에 적용할 수 있습니다.

주로 하나의 경로에서 미들웨어를 적용할 때 사용합니다.

전달된 인자의 순서대로 동작합니다.

```js
app.use(middleware1, middlware2, ...);

app.use("/admin", auth, adminRouter);

app.get("/", logger, (req, res, next) => {
  res.send("Hello Express");
});
```

---

## 여러 미들웨어를 활용한 예제

```js
const express = require("express");

const app = express();

// 애플리케이션 미들웨어 연결
app.use((req, res, next) => {
  console.log("모든 요청에 다 실행됩니다.");

  next();
});

// 라우터 미들웨어 연결

// /error 경로에 get 요청 올때 동작
app.get(
  "/error",
  (req, res, next) => {
    next(); // next()에 인수가 없다면, 바로 다음 미들웨어 함수로 넘어가게 된다.
  },
  (req, res) => {
    // 미들웨어 서브스택으로 여러개 넣어줘도 됩니다.
    try {
      ...
    } catch (err) {
      next(err); // next()에 인수가 있다면, 에러 처리 미들웨어 넘어갑니다.
    }
  }
);

// /about 경로에 get 요청 올때 동작
app.get("/about", (req, res) => {
  res.send("Hello, about");
});

// 주소 부분에는 정규표현식, : (콜론)을 사용한 와일드 카드도 적용이 가능합니다.
// :변수명 정도로 사용할 수 있습니다.
// 미들웨어는 위에서부터 처리되므로 와일드 카드를 사용할때는 다른 라우터 보다 뒤에 적어주는 것이 좋습니다.
app.get("/category/:name", (req, res) => {
  res.send(`Hello, ${req.params.name}`);
});

// 모든 종류의 get요청이 올때 동작
app.get("*", (req, res) => {
  res.send("Hello, get !!");
});

// 에러 처리 미들웨어 연결
app.use((err, req, res, next) => {
  // 에러 미들웨어는 인자는 반드시 4개 선언
  console.error(err);

  res.status(500).send(err.message);
});

app.listen(3000, () => {
  console.log("3000번 포트에서 대기 중.");
});
```
