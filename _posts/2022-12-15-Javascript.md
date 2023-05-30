---
title: Javascript 동작 원리
author: Psmin
data: 2022-12-15 16:18:32 +0900
categories: [Knowledge, Javascript]
tags: [Javascript]
---

# Javascript의 동작 원리에 대해 알아보자.

---

## Javascript Engine

JS engine은 자바스크립트 코드를 실행하는 프로그램 또는 **_인터프리터_**입니다.

> 인터프리터란?
> 프로그래밍 언어의 소스를 바로 실행하는 컴퓨터 프로그램 또는 환경

대표적인 예로는 **_Google의 V8 엔진_**이 있습니다.

- 구성 요소
  - **메모리 힙** (**_Memory Heap_**) : 메모리 할당이 일어나는 장소 (선언한 변수, 함수, 객체 등 모든 메모리 할당이 발생합니다.)
  - **콜 스택** (**_Call Stack_**) : 코드가 실행되면서 함수의 호출을 스택 형식으로 저장하는 자료구조

<u>Call Stack이 하나</u>이기 때문에 `동기적으로 한 가지 일만 처리` 할 수 있습니다.  
따라서, Javascript는 `Single-Thread 기반`의 언어입니다.

- 간단한 Call Stack 예제

  ```js
  function multiply(x, y) {
    return x * y;
  }

  function printSquare(x) {
    var s = multiply(x, x);
    console.log(s);
  }

  printSquare(5);
  ```

  위 코드가 실행되면서 Call Stack은 아래의 사진처럼 변합니다.

  ![CallStack](/assets/img/call-stack.png){: .w-80 .normal}

---

{: .prompt-tip}

> 그렇다면 우리는 어떻게 Javascript로 비동기 처리를 할 수 있을까요?

## Javscript Runtime

JS 엔진은 Single-Thread 기반이기 때문에 수행중인 작업이 끝날 때 까지 기다려야합니다.

<u>네트워크 요청과 같은 비동기 함수가 동기적으로 이루어지는 함수로 만들어졌다면, 어떤 일이 일어날까요?</u>

네트워크 요청이 다른 서버로 보내지고, 컴퓨터는 응답 받기를 기다리며 느려질 것입니다.  
그 사이에 클릭이나, 다른 요소가 렌더링이 되어져야 하는게 있더라도, 스택은 네트워크 요청 함수에 **_블락킹_** 되어있으므로, 아무 일도 일어나지 않게 됩니다.

즉, 사용자는 해당 작업이 끝날 때까지 아무 일도 못하고 기다려야하는 상황이 벌어집니다.

이러한 문제는 **_비동기 콜백_**을 사용함으로써 해결됩니다. 비동기 요청은 자바스크립트 엔진을 구동하는 환경인 **_브라우저_**나 **_Node.js_**에서 처리됩니다.

---

### 웹 브라우저 환경의 구조

![js-browser](/assets/img/js-browser-logic.png){: .w-80 .normal}

- JS 엔진

  - Heap : 메모리 할당이 일어나는 장소, 참조 타입(객체 등) 데이터가 저장됩니다.
  - Call Stack : 원시 타입(숫자 등) 데이터가 저장됩니다.

- Web Api : 웹 브라우저에 구현된 API입니다. (Ajax 요청, setTimeout(), DOM event 등)

  > Call Stack에서 실행된 비동기 함수들은 모두 Web API를 호출합니다.
  > Web API는 콜백 함수를 Callback Queue에 넣습니다.

- Event Loop : Call Stack과 Callback Queue들을 감시하며 Call Stack이 비어있다면 Callback Queue에서 Call Stack으로 콜백 함수를 넘겨주는 작업을 합니다.  
  (이와 같은 반복적인 행동을 틱(tick)이라고 부릅니다.)

  > Event Loop 3가지
  >
  > 1. window event loop
  > 2. worker event loop
  > 3. worklet event loop

- Callback Queue : 비동기적으로 실행된 콜백 함수가 보관되는 곳입니다.
  (Call Stack이 비어졌을 때 먼저 대기열에 들어온 순서대로 수행됩니다.)

  > Callback Queue 3가지
  >
  > 1. Task Queue : setTimeout, setInterval, setImmediate 등의 콜백 함수 저장
  > 2. MicroTask Queue : Promise, Object.observe 등의 콜백 함수가 저장 (ES6)
  > 3. Animation Frames : requestAnimationFrame 콜백 함수가 저장
  >
  > MicroTask Queue -> Animation Frames -> Task Queue 순서로 처리합니다.

---

### Node.js의 구조

![js-nodejs](/assets//img/js-nodejs-logic.png){: .w-100 .normal}

Node.js 는 비동기 IO를 지원하기 위해 <kbd>libuv</kbd> 라이브러리를 사용한다.  
libuv는 Event Loop를 제공한다.

---

## JS 런타임 실행 예제

```js
function foo() {
  console.log("a");
}

function foo2() {
  console.log("b");
}

foo();
setTimeout(function () {
  console.log("c");
}, 2000);
foo2();
```

```js
a;
b;
c;
```

실행 과정

1. foo() 함수가 실행 되고 Call stack에 쌓입니다.
2. foo() 함수 안의 console.log()가 Call stack에 쌓입니다.
3. a 출력
4. console.log()가 Call Stack에서 빠지고, foo()함수도 Call Stack에서 빠집니다.
5. setTimeOut()함수가 Call Stack에 쌓입니다.
6. 비동기 함수인 setTimeOut()은 Web API가 처리하도록 보냅니다.
7. Web API가 setTimeOut()함수를 처리해 Callback Queue에 보내는 동안 foo2()함수가 Call Stack에 쌓입니다.
8. foo2()함수 안의 console.log()가 Call Stack에 쌓입니다.
9. b 출력
10. console.log()가 Call Stack에서 빠지고, foo2()함수도 Call Stack에서 빠집니다.
11. Call Stack이 비어 있으므로, Callback Queue에 있던 함수를 Call Stack으로 보냅니다.
12. console.log()가 Call Stack에 쌓입니다.
13. c 출력
14. console.log()가 Call Stack에서 빠집니다.
15. 프로그램 종료

---

## 결론

Javascript 엔진은 Single-thread 방식으로 동작한다 Call Stack에 함수가 쌓이고 LIFO으로 처리하는 방식입니다.
비동기 처리를 위해 브라우저의 Web API, Node.js의 libuv 라이브러리과 Callback Queue, Event Loop 등을 이용합니다.
브라우저와 Node.js는 멀티 스레드로 동작합니다.

> Node.js는 Event Loop가 하나이므로 싱글 스레드라고도 말합니다. (추가 공부 필요)

---

## 참조

- <https://bbangson.tistory.com/89>
- <https://velog.io/@hang_kem_0531/JS-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC>
- <https://velog.io/@yejineee/%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84%EC%99%80-%ED%83%9C%EC%8A%A4%ED%81%AC-%ED%81%90-%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C-%ED%83%9C%EC%8A%A4%ED%81%AC-%EB%A7%A4%ED%81%AC%EB%A1%9C-%ED%83%9C%EC%8A%A4%ED%81%AC-g6f0joxx>
- <https://zereight.tistory.com/855>
- <https://talkwithcode.tistory.com/89>
