---
title: 변수 선언 및 할당
author: Psmin
data: 2022-12-18 14:28:32 +0900
categories: [Knowledge, Javascript]
tags: [Hoisting, Scope]
---

# 변수의 선언 및 할당 과정, Hoisting, Scope 개념에 대해 알아보자.

---

## 변수 (Variable)

Javascript에서 변수는 하나의 값을 저장하기 위해 확보한 메모리 공간을 식별하기 위해 붙인 이름을 말합니다.

변수명은 변수의 값이 아닌 메모리 주소를 기억하고 있습니다.  
JS engine은 변수명과 매핑된 메모리 주소를 통해 저장된 변수 값을 반환해줍니다.

이러한 변수에 값을 저장하는 것을 <kbd>할당 (assignment)</kbd>,  
변수에 저장된 변수 값을 읽어 들이는 것을 <kbd>참조 (reference)</kbd>,  
변수명을 JS engine에 알리는 것을 <kbd>선언 (declaration)</kbd> 이라고 합니다.

---

### 변수 선언

변수의 선언은 <kbd>var</kbd>, <kbd>const</kbd>, <kbd>let</kbd> 키워드로 사용합니다.  
선언 키워드는 아래에서 자세히 살펴보겠습니다.

자바스크립트에서 변수 선언은 <kbd>선언 -> 초기화</kbd> 를 통해 수행됩니다.

- 선언 단계 : 변수명을 등록하여 JS engine에 변수의 존재를 알립니다.
- 초기화 단계 : 값을 저장하기 위해 메모리 공간을 확보하고 undefined를 할당해 초기화합니다.

- 변수 선언 과정
  ```js
  var tmp;
  ```
  키워드를 통해 변수를 선언하면 변수명에 메모리 공간을 확보하고 undefined로 초기화합니다.  
  var 키워드를 이용한 변수 선언은 선언 단계와 초기화 단계가 동시에 진행됩니다.  
  (여기서 변수는 메모리 공간을 가르킵니다.)

---

### 변수 할당

변수의 할당은 <kbd>=</kbd> 연산자를 사용합니다.

```js
var tmp; // 변수 선언
tmp = "content"; // 변수 할당

var tmp = "content"; // 변수 선언과 할당을 동시에
```

변수 선언과 할당은 동시에 할 수 있지만, 두 개의 실행 시점은 다릅니다.  
변수 선언이 런타임 이전에 실행되고, 값의 할당은 소스코드가 순차적으로 실행되는 런타임에 실행됩니다.

---

### 변수 Hoisting

변수 선언문이 코드의 맨 위로 끌어올려진 것처럼 동작하는 JS의 특징입니다.

런타임 이전에 선언된 변수들을 실행 컨텍스트에 먼저 등록하기 때문에 나타나는 특징입니다.

```js
console.log(tmp); // undefined

var tmp = "content";

console.log(tmp); // content
```

첫번째 console문은 아직 tmp가 선언되기 전이므로 참조 에러가 발생할 것 같지만 호이스팅의 특징으로 undefined가 출력됩니다.

---

### 함수 Hoisting

함수 선언문에도 위와 같이 호이스팅이 적용됩니다.

```js
// 함수 참조
console.dir(add); // f add(x, y)
console.dir(sub); // undefined

// 함수 호출
console.log(add(2, 5)); // 7
console.log(sub(2, 5)); // Uncaught TypeError: sub is not a function

// 함수 선언문
function add(x, y) {
  return x + y;
}

// 함수 표현식
var sub = function (x, y) {
  return x + y;
};
```

함수 선언문은 함수 자체를 호이스팅 할 수 있지만, 함수 표현식은 변수 호이스팅처럼 undefined로 초기화되는 것을 볼 수 있습니다.

> 만약, 함수 호이스팅을 사용하려면 선언문으로 작성해야합니다.

---

## 변수 선언 키워드

---

### var

ES6 이전에 사용했던 키워드입니다.

- **특징**

  - var 변수는 호이스팅이 가능하며 <kbd>undefined</kbd>로 초기화됩니다.

  - var 변수는 재선언 가능하며, 업데이트 될 수 있습니다.

    ```js
    var tmp = "hi";
    var tmp = "Hello";

    var tmp = "hi";
    tmp = "Hello";
    ```

  - var 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정하는 함수 레벨 스코프를 따릅니다.

    ```js
    var a = 1;

    if (true) {
      var a = 5;
    }

    console.log(a); // 5
    ```

- **var 키워드의 문제점**
  - 변수 중복 선언 가능하여, 예기치 못한 값을 반환할 수 있다.
  - 함수 레벨 스코프로 인해 함수 외부에서 선언한 변수는 모두 전역 변수로 된다.
  - 변수 선언문 이전에 변수를 참조하면 언제나 undefined를 반환한다.

---

### let

var의 문제점을 해결하기 위해 ES6에서 나온 선언 키워드입니다.

- **특징**

  - let 변수는 호이스팅은 가능하지만 초기화되지 않으며, 선언 이전에 사용하려고하면 참조 오류가 발생합니다.

  - let 변수는 재선언은 불가능하며, 업데이트 될 수 있습니다.

    ```js
    let tmp = "hi";
    let tmp = "Hello"; // error: Identifier 'tmp' has already been declared

    let tmp = "hi";
    tmp = "Hello";
    ```

  - let 변수는 블록 레벨 스코프를 따릅니다.

    > 블록 레벨 스코프란 <kbd>{}</kbd>로 바인딩된 모든 코드 블록(if, for, while, try/catch 등)이 지역 스코프를 만드는 특성입니다.

    ```js
    var a = 1;

    if (true) {
      let b = 5;
    }

    console.log(a); // 1
    console.log(b); // b is not defined
    ```

---

### const

let 키워드처럼 ES6에서 나온 선언 키워드입니다.
const 키워드로 선언된 변수는 항상 일정한 상수 값을 유지합니다.

- **특징**

  - const 변수도 let 변수와 마찬가지로 호이스팅은 가능하지만 초기화되지 않습니다.

  - const 변수는 재선언이 불가능하며, 재할당도 할 수 없습니다.  
    따라서, const 변수는 항상 선언과 할당이 동시에 이루어져야합니다.

    ```js
    const tmp = "hi";
    const tmp = "Hello"; // error: Identifier 'tmp' has already been declared

    const tmp = "hi";
    tmp = "Hello"; // error: Assignment to constant variable.
    ```

  - const 변수는 let 변수와 마찬가지로 블록 레벨 스코프를 따릅니다.

---

### 정리

![var-let-const](/assets/img/var-let-const.png){: .normal}

---

## 결론

구글의 자바스크립트 스타일 가이드에는 다음의 내용이 있습니다.

- 모든 지역 변수를 const나 let으로 선언해라.
- 변수를 재할당하지 않는 한, 기본적으로 const를 사용해라.
- var 키워드는 절대 사용하지 마라.

> 이유

- 코드를 보는 것만으로 어떤 변수가 변경 불가능한 지, 재할당 가능한 지를 let과 const로 구분할 수 있습니다.
- Scope 측면에서 더욱 신중하게 변수를 배치할 수 있습니다.
- 보다 좋은 메모리 관리로 성능이 올라갑니다.

앞으로 코드를 작성하며 변수를 선언할 때, 해당 변수가 앞으로 변경할 값인지를 미리 생각하여 let과 const만으로 구현하도록 노력해야겠다.

---

## 참조

- <https://minsoftk.tistory.com/m/84>
- <https://www.howdy-mj.me/javascript/var-let-const>
- <https://espania.tistory.com/m/404>
- <https://escapefromcoding.tistory.com/307>
- <https://www.freecodecamp.org/korean/news/var-let-constyi-caijeomeun/>
