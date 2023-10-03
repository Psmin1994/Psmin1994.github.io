---
title: async / await
author: Psmin
data: 2023-03-14 04:11:12 +0900
categories: [Knowledge, Javascript]
tags: [Javascript, Async/Await]
---

# Javascript 에서의 Async / Await 에 대해 알아보자.

---

## Async / Await

async / await는 Promise를 쉽게 사용할 수 있도록 도와주는 Promise의 `Syntatic sugar`입니다.  
(Syntatic sugar란 가독성을 좋게 만드는 것을 말합니다.)

---

## 기본 문법

```js
async function 함수명() {
  await 비동기_처리_메소드명();
}
```

1. 함수 앞에 <kbd>async</kbd> 를 붙여줍니다.
2. 내부 로직 중 <u>HTTP 통신을 하는 비동기 처리 코드</u> 앞에 <kbd>await</kbd> 를 붙여줍니다.
3. 비동기 처리 코드는 Promise 객체를 반환해야 합니다.  
   (일반적으로 Axios, Fetch 등을 활용합니다.)  
   <br/>

---

## Async

async 키워드는 함수 앞에 사용합니다.  
async가 붙은 함수는 항상 Promise를 반환합니다.  
Promise가 아닌 값을 반환하는 함수여도 resolved Promise로 감싸서 Promise가 반환되도록 합니다.

```js
async function f() {
  return 1;
}

f().then(alert); // 1

async function f() {
  return Promise.resolve(1);
}

f().then(alert); // 1
```

---

## await

await는 async 함수 안에서만 동작합니다.  
Promise가 처리될 때까지 기다리는 역할을 합니다.  
Promise가 처리를 기다리는 역할이므로 다른 일(이벤트 처리, 다른 스크립트 실행 등)은 비동기로 처리 가능합니다.

```js
async function f() {
  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("완료!"), 1000);
  });

  let result = await promise; // 프라미스가 이행될 때까지 기다립니다.

  alert(result); // "완료!"
}

f();
```

---

## 에러 처리

Promise가 거부되면 `throw`문을 작성한 것 처럼 에러를 던집니다.

```js
// 두 코드는 같습니다.
async function f() {
  await Promise.reject(new Error("에러 발생!"));
}

async function f() {
  throw new Error("에러 발생!");
}
```

await가 던진 에러는 try / catch 문으로 잡을 수 있습니다.

```js
  import Axios from 'axios'

  const axios = Axios.create(
    baseURL: 'https://some-domain.com/api',
    timeout: 1000,
    headers: {'X-Custom-Header': 'foobar'}
  )

  async function f() {
    try {
      let response = await axios.get('/user/1');
      let user = await response.json();
    } catch(err) {
      // axios와 response.json에서 발행한 에러 모두를 여기서 잡습니다.
      alert(err);
    }
  }

  f();
```

`await`가 대기 처리를 해주기 때문에 `.then()`메소드는 필요하지 않습니다.

즉, `async/await`를 사용하면 `promise.then/catch`가 거의 필요 없습니다.

하지만 가장 바깥 스코프에서 비동기 처리가 필요할 때처럼 `promise.then/catch`를 써야하는 경우가 존재합니다.

async/await가 Promise를 기반으로 한다는 사실은 알고 계셔야 합니다.
