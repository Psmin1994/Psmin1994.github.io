---
title: 동기, 비동기 처리
author: Psmin
data: 2023-03-09 20:59:22 +0900
categories: [Knowledge, Javascript]
tags: [Javascript, Asynchronous]
---

# Javascript에서의 동기, 비동기에 대해 알아보자.

---

## 동기 처리 (Synchronous)

우선순위 작업이 끝날 때까지 기다리는동안 <u>준비상태가 되어 다음 작업을 할 수 없는 특징</u>을 말합니다.

즉, **_직렬적_**으로 작업을 수행하기 때문에 요청과 동시에 그 응답이 요청을 요구한 자리에 바로 주어져야하므로  
응답이 올 때 까지 기다려야 한다는 단점이 있다.

![synchronous](/assets/img/synchronous.png){: .normal .w-75}

---

## 동기 처리 예시

```js
const second = () => {
  console.log("second");
};

console.log("first");
second();
console.log("third");
```

```
  first
  second
  third
```

---

## 비동기 처리 (Asynchronous)

특정 코드의 연산이 끝날 때까지 <u>코드의 실행을 멈추지 않고 다음 코드를 먼저 실행하는 특징</u>을 말합니다.

즉, **_병렬적_**으로 작업을 수행하며 동시에 여러 작업을 처리 할 수 있고 기다리는 과정에서 새로운 요청을 보낼 수 있는 장점이 있습니다.

![Asynchronous](/assets/img/asynchronous.png){: .normal .w-75}

---

## 비동기 처리 예시

---

### `setTimeout 함수`

사용법
: setTimeout(호출 될 함수, 기다릴 시간(ms))

```js
  console.log('first')';

  setTimeout(() = > {
    console.log('second')';
  }, 2000);

  console.log('third')';
```

```
  first
  third
  second
```

---

### `callback 함수`

**_callback 함수_**란 함수 안에서 실행하는 또 다른 함수를 말합니다.  
Javascript에서 함수는 함수의 <u>매개변수</u>로 전달할 수 있습니다.

즉, callback 함수는 특정함수에 <u>인자</u>로 사용되는 함수를 말합니다.

인자로 받은 callback 함수는 다른 작업들을 하다가 <u>필요한 시점에 호출</u>해서 사용할 수 있습니다.

```js
const func = (callback) => {
  console.log("first");
  callback();
  console.log("third");
};
func(() => {
  console.log("second");
});
```

---

### `Promise 객체 (ES6)`

**_Promise_**란 비동기 작업이 종료된 이후에 결과 값과 실패 사유 등을 처리하기 위한 함수들을 설정하는 모듈입니다.

- **Promise 생성자 함수**  
   <br/>
  **_Promise_**는 객체이기 때문에 <u>Promise 생성자 함수</u>를 호출하여 인스턴스화합니다.  
   **_Promise 생성자 함수_**는 비동기 작업을 수행할 <u>callback 함수</u>를 인자로 전달 받습니다.  
   (resolve, reject 함수)

  ```js
    // Promise 객체의 생성
    const promise = new Promise((resolve, reject) => {
      // 비동기 작업을 수행한다.
      if (/* 비동기 작업 수행 성공 */) {
        resolve('result');
      } else { /* 비동기 작업 수행 실패 */
        reject('fail reason');
      }
    });
  ```

- **Promise의 상태정보**  
  Promise는 비동기 작업 수행의 성공, 실패에 따라 <u>상태 정보</u>를 갖습니다.

  |   상태    |      의미      |                  구현                  |
  | :-------: | :------------: | :------------------------------------: |
  |  pending  | 비동기 수행 전 | resolve 또는 reject 함수 호출 전 상태  |
  | fulfilled |  비동기 성공   |       resolve 함수가 호출된 상태       |
  | rejected  |  비동기 실패   |       reject 함수가 호출된 상태        |
  |  settled  |  비동기 완료   | resolve 또는 reject 함수가 호출된 상태 |

- **후속 처리 메소드**  
  **_Promise 객체의 후속 처리 Method (then, catch)_**를 총해 <u>비동기 처리 결과, 에러 메세지</u>를 전달 받아 처리합니다.

  - **then 메소드**

    - then 메소드는 2개의 callback 함수를 인자로 전달 받습니다. - 첫 번째 인자는 성공 시 실행됩니다. ( 즉, fulfilled 상태, resolve 함수가 호출된 경우)
    - 두 번쨰 인자는 실패 시 실행됩니다. ( 즉, rejected 상태, reject 함수가 호출된 경우)

  - **catch 메소드**
    - catch 메소드는 비동기 처리와 then 메소드 실행 중 예외 발생 시 호출됩니다.

  ```js
  const promise = new Promise((resolve, reject) => {
    // 실제로는 여기서 XHR이나 HTML5 API를 사용할 것입니다.
    setTimeout(() => {
      resolve("success");
      // reject(new Error("network error")); 비동기 처리 실패 시
    }, 2000);
  });
  promise
    .then((val) => {
      console.log(val);
    })
    .catch((error) => {
      console.lot(error);
    });
  ```

- **Promise의 에러 처리**  
  Promise를 이용한 에러 처리 방법은 2가지가 있습니다.

  - then 메소드의 두 번째 인자로 에러 처리하는 방법

  ```js
  const promise = () => {
    new Promise((resolve, reject) => {
      let a = 1;

      if (a == 3) {
        resolve("success");
      } else {
        reject("failed");
      }
    });
  };

  promise().then(
    (message) => {
      console.log("This is in the then " + message);
    },
    (error) => {
      console.log("This is in the then " + error);
    }
  );
  ```

  <br/>

  - catch 메소드를 이용해 에러 처리하는 방법

  ```js
  const promise = () => {
    new Promise((resolve, reject) => {
      let a = 1;

      if (a == 3) {
        resolve("success");
      } else {
        reject("failed");
      }
    });
  };

  promise()
    .then((message) => {
      console.log("This is in the then " + message);
    })
    .catch((error) => {
      console.log("This is in the then " + error);
    });
  ```

---

### `catch 메소드를 사용하는 이유`

**catch 메소드**를 모든 then 메소드를 호출한 이후에 호출하면 <u>비동기 처리에서 발생한 에러(reject 상태)</u>뿐만 아니라 <u>then 메서드 내부</u>에서 발생한 에러까지 처리할 수 있습니다.

```js
const promise = () =>
  new Promise((resolve, reject) => {
    let a = 1;

    if (a == 3) {
      resolve("success");
    } else {
      reject("failed");
    }
  });

promise()
  .then((message) => {
    console.xxx("This is in the then " + message);
  })
  .catch((error) => {
    console.log("This is in the then " + error);
  });
```

```
  TypeError: console.xxx is not a function
```
