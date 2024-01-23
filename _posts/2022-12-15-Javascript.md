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
>
> 프로그래밍 언어의 소스를 바로 실행하는 컴퓨터 프로그램 또는 환경

인터프리터 방식은 프로그램 전체를 번역 후 실행하는 컴파일러와 달리 프로그램을 1줄씩 번역하는 방식입니다.

인터프리터 방식 장점

- 쉽게 실시간으로 수정이 가능하다.
- 디버깅 과정이 가볍다.

인터프리터 방식 단점

- 컴파일된 실행 파일보다 **_실행속도가 느리다._**
- 매 실행마다 직접 코드를 구동하기 때문에 **_인터프리터가 있어야한다_**

대표적인 예로는 **_Google의 V8 엔진_**이 있습니다.

- 구성 요소

  - **메모리 힙** (**_Memory Heap_**)  
    : 메모리 할당이 일어나는 장소 (선언한 변수, 함수, 객체 등 모든 메모리 할당이 발생합니다.)

  - **콜 스택** (**_Call Stack_**)  
    : 코드가 실행되면서 함수의 호출을 스택 형식으로 저장하는 자료구조

<u>Call Stack이 하나</u>이기 때문에 **_동기적으로 한 가지 일만 처리_** 할 수 있습니다.

즉, Javascript는 **_Single-Thread 기반_**의 언어입니다.

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

  ![CallStack](/assets/img/call-stack.png){: .normal}

---

{: .prompt-tip}

> 그렇다면 우리는 어떻게 Javascript로 비동기 처리를 할 수 있을까요?

## Javscript 런타임

자바스크립트 런타임은 프로그래밍 언어가 구동되는 환경을 말합니다.

JS 엔진은 **_Single-Thread 기반_**이기 때문에 수행중인 작업이 끝날 때까지 기다려야합니다.

<u>네트워크 요청과 같은 비동기 함수가 동기적으로 이루어지는 함수로 만들어졌다면, 어떤 일이 일어날까요?</u>

네트워크 요청이 다른 서버로 보내지고, 컴퓨터는 응답 받기를 기다리며 느려질 것입니다.

그 사이에 클릭이나, 다른 요소가 렌더링이 되어져야 하는게 있더라도, 스택은 네트워크 요청 함수에 **_블락킹_** 되어있으므로, 아무 일도 일어나지 않게 됩니다.

즉, <u>사용자는 해당 작업이 끝날 때까지 아무 일도 못하고 기다려야하는 상황</u>이 벌어집니다.

이러한 문제는 JavaScript 런타임에서 제공하는 기능으로 해결합니다.

대표적인 JS 런타임인 **_웹 브라우저_**와 **_Node.js_**에 대해 가볍게 알아보겠습니다.

---

### 웹 브라우저 환경의 구조

![js-browser](/assets/img/js-browser-logic.png){: .normal}

- **_JS 엔진_**

  - Heap : 메모리 할당이 일어나는 장소, 참조 타입(객체 등) 데이터가 저장됩니다.
  - Call Stack : 원시 타입(숫자 등) 데이터가 저장됩니다.

- **_Web Api_**  
  웹 브라우저에 구현된 API로 위에서 말한 비동기 등을 처리합니다.  
  (Ajax 요청, setTimeout(), DOM event 등)

  > Call Stack에서 실행된 비동기 함수들은 모두 Web API를 호출합니다.  
  > 그 다음, Web API는 콜백 함수를 Callback Queue에 넣습니다.

- **_Event Loop_**  
  **_Call Stack_**과 **_Callback Queue_**들을 감시하며 <u>Call Stack이 비어있다면 Callback Queue에서 Call Stack으로 콜백 함수를 넘겨주는 작업</u>을 합니다.  
  (이와 같은 반복적인 행동을 틱(tick)이라고 부릅니다.)

  > Event Loop 3가지
  >
  > 1. window event loop
  > 2. worker event loop
  > 3. worklet event loop

- **_Callback Queue_**  
  비동기적으로 실행된 콜백 함수가 보관되는 곳입니다.  
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

![js-nodejs](/assets//img/js-nodejs-logic.png){: 0 .normal}

Node.js 는 비동기 IO를 지원하기 위해 <kbd>libuv</kbd> 라이브러리를 사용합니다.  
**_libuv_**는 Event Loop를 제공합니다.

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

```console
<!-- 실행 결과 -->
a
b
c
```

실행 과정

1. **_foo() 함수_**가 실행 되고 **_Call stack_**에 쌓입니다.
2. **_foo() 함수_** 안의 **_console.log()_**가 **_Call stack_**에 쌓입니다.
3. a 출력
4. **_console.log()_**가 **_Call Stack_**에서 빠지고, **_foo()함수_**도 **_Call Stack_**에서 빠집니다.
5. **_setTimeOut()함수_**가 **_Call Stack_**에 쌓입니다.
6. 비동기 함수인 **_setTimeOut()_**은 **_Web API_**가 처리하도록 보냅니다.
7. **_Web API_**가 **_setTimeOut()함수_**를 처리해 **_Callback Queue_**에 보내는 동안 **_foo2()함수_**가 **_Call Stack_**에 쌓입니다.
8. **_foo2()함수_** 안의 **_console.log()_**가 **_Call Stack_**에 쌓입니다.
9. b 출력
10. **_console.log()_**가 **_Call Stack_**에서 빠지고, **_foo2()함수_**도 **_Call Stack_**에서 빠집니다.
11. **_Call Stack_**이 비어 있으므로, **_Callback Queue_**에 있던 함수를 **_Call Stack_**으로 보냅니다.
12. **_console.log()_**가 **_Call Stack_**에 쌓입니다.
13. c 출력
14. **_console.log()_**가 **_Call Stack_**에서 빠집니다.
15. 프로그램 종료

---

## 결론

**_JS engine_**은 **_Single-thread 방식_**으로 **_Call Stack에 함수가 쌓이고 LIFO으로 처리하는 방식_**입니다.

비동기 처리를 위해서 브라우저의 **_Web API_**, Node.js의 **_libuv 라이브러리_**와 같은 런타임에서 제공하는 기능을 이용합니다.

브라우저와 Node.js는 멀티 스레드로 동작합니다.

> Node.js는 Event Loop가 하나이므로 싱글 스레드라고도 말합니다. (추가 공부 필요)

---

## 참조

- <https://bbangson.tistory.com/89>
- <https://velog.io/@hang_kem_0531/JS-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC>
- <https://velog.io/@yejineee/%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%A3%A8%ED%94%84%EC%99%80-%ED%83%9C%EC%8A%A4%ED%81%AC-%ED%81%90-%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C-%ED%83%9C%EC%8A%A4%ED%81%AC-%EB%A7%A4%ED%81%AC%EB%A1%9C-%ED%83%9C%EC%8A%A4%ED%81%AC-g6f0joxx>
- <https://zereight.tistory.com/855>
- <https://talkwithcode.tistory.com/89>
