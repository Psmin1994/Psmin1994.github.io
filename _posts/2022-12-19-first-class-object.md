---
title: 일급 객체 (First Class Object)
author: Psmin
data: 2022-12-19 20:30:31 +0900
categories: [Javascript]
tags: [Javascript]
---

## 일급 객체란?

다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체를 말합니다.  
즉, <u>함수에 인자로 넘기기</u>, <u>변수에 대입하기</u>와 같은 연산을 지원할 때 일급 객체라고합니다.

---

### 조건

1. <u>무명 리터럴</u>로 생성할 수 있다.  
   (함수 표현식을 의미하며 값으로 표현될 수 있기 때문에 함수 표현식으로 사용가능합니다.)
   ```javascript
   var test = function () {
     return "test";
   };
   ```
2. <u>변수에 저장</u>할 수 있다.
   ```javascript
   var test = function () {
     return "test";
   };
   console.log(test()); // test
   ```
3. <u>함수의 실제 매개변수</u>가 될 수 있다.

   ```javascript
   var test = function (func) {
     //func 은 매개변수 (parameter)
     func();
   };

   test(function () {
     console.log("test");
   });
   ```

4. <u>함수의 리턴 값</u>이 될 수 있다.

   ```javascript
   function test() {
     return function () {
       // return 값에 함수
       console.log("test");
     };
   }

   var a = test();
   a();
   ```

---

### 함수 객체

`Javascipt`에서 함수를 객체와 동일하게 사용할 수 있습니다.  
위에서 말했듯이 **_변수, 배열 요소, 객체의 값_**으로 **_할당_**할 수 있으며 다른 함수의 **_인수_**로 전달되거나 함수에서 **_반환_**될 수 있습니다.  
또한, 이러한 특징은 **_함수형 프로그래밍_**을 가능하게 합니다.

- 함수는 `객체의 인스턴스`이다.  
  함수는 실제로 객체의 인스턴스이며 함수를 정의하는 것은 객체를 만드는 것입니다.  
  함수는 <kbd>Object.prototype</kbd> 객체의 프로퍼티를 상속 받아 고유의 프로퍼티를 사용할 수 있습니다.

  ```js
    function test() {
        console.log('test');
    }

    console.log(type of test); // function
    console.log(test instanceof Object) // true
  ```

---

### 고차 함수 (Higher Order Function)

함수를 인자(parameter)로 받을 수 있고, 함수의 형태로 return할 수 있습니다.

- 예제

  ```javascript
    function test(n) {
        return n * 2;
    }

    function hofunc(n1, func) { // 다른 함수를 인수로 전달 받을 수 있다.
        const tmp = func(n1);
        return finction (n2) { // return 값으로 함수를 지정할 수 있다.
            return n2 * tmp;
        };
    }

    const result = hofunc(3, test)(2);
    // hofunc(3,test)는 return값으로 받은 함수입니다.
    // 따라서, 호출 연산자 ()를 사용할 수 있습니다.

    console.log(result); // 12
  ```

---

### 콜백 함수 (Callback Function)

다른 함수 (Caller)의 인자로 전달되는 함수를 말합니다.  
인자를 넘겨 받는 함수(Caller)는 Callback 함수를 필요에 따라 즉시 실행하거나 비동기적으로 실행 할 수 있습니다.

---

### 커리 함수 (Curry Function)

함수를 리턴하는 함수를 부르는 용어입니다.
고차 함수 중에서도 함수를 리턴하는 모양의 함수만을 지칭합니다.

---

### 추상화 (Abstraction)

복잡한 내용을 압축해서 **_핵심_**만 추출한 상태로 만드는 것입니다.  
CPU는 0과 1만 이해합니다. 코드가 해석되는 복잡한 것들은 **_자바스크립트 엔진_**이 대신해줍니다.  
우리는 **_자바스크립트 문법(syntax)_**을 올바르게 사용하는 것만으로, 다양한 프로그램을 비교적 쉽게 작성할 수 있습니다.  
이처럼 고민거리가 줄어들고, 문제 해결이 더 쉬워지는 것이 추상화의 장점입니다.
즉, 자바스크립트 포함한 많은 프로그래밍 언어 또한 추상화의 결과입니다.

- `고차 함수를 사용하는 이유`  
  추상화의 관점에서 **_함수_**는 **_논리(logic)_**의 묶음입니다.

  함수는 **_값_**을 전달 받고 **_값_**을 **_return_**한니다. 값에 대한 복잡한 로직은 감춰져있습니다.  
  즉, 값 수준에서의 추상화입니다.

  **고차 함수**는 함수를 통해 얻은 추상화를 **_사고(thick)_**의 추상화 수준으로 끌어올립니다.  
  고차 함수는 **_함수_**를 전달받거나 **_함수_**를 **_return_**합니다. 함수에 대한 복잡한 로직은 감춰있다.  
  즉, 사고 수준에서의 추상화입니다.

  추상화의 수준이 높아진 만큼 생선성도 향상됩니다.
