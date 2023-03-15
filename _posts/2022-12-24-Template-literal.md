---
title: 템플릿 리터털, 화살표 함수
author: Psmin
data: 2022-12-24 14:21:23 +0900
categories: [Nodejs]
tags: [Template Literal, Arrow Function]
---

# 템플릿 리터럴과 화살표 함수에 대해 알아보자.

- Javascript에서의 this 바인딩에 대해 알아보자.

---

## 템플릿 리터럴 Template Literal

템플릿 리터럴이란 내장된 표현식을 허용하는 문자열 리터럴입니다.  
여러 줄로 이뤄진 문자열과 문자 보간기능을 사용할 수 있습니다.

백틱(\` \`) 을 사용해 표현합니다.  
또한, 플레이스 홀더를 이용하여 표현식(`$ {expression}`)을 넣을 수 있습니다.  
플레이스 홀더 안에서의 표현식과 그 사이의 텍스트는 함께 함수로 전달되고 함수는 단순히 해당 부분을 단일 문자열로 연결시켜 줍니다.

```js
//기존 문자열 출력 방식
let data = expression;

console.log("data is" + data);

//변경
let data = expression;

console.log(`data is ${data}`);
```

---

## 화살표 함수 Arrow Function

화살표 함수란 본래 JavaScript에서 함수를 선언할 때 사용했던 <kbd>function</kbd> 이란 키워드 대신 ES6에서 도입한 함수를 선언하는 새로운 문법입니다.  
이름에서 알 수 있듯이 화살표 (=>) 표시를 사용해서 함수를 선언하는 방법입니다.

```js
// 기존 함수
const foo = function () {
  console.log("기존 함수");
};

// 화살표 함수
const foo = () => console.log("화살표 함수");
```

화살표 함수를 사용하면 코드를 좀 더 간결하게 쓸 수 있다는 장점이 있습니다.  
그러나 화살표 함수는 두 가지 제약이 있습니다.

- 무조건 익명함수로만 사용할 수 있다.
- 메소드나 생성자 함수로 사용할 수 없다.

{: .prompt-info}

> 단점이 더 많은 느낌입니다. 코드가 간결해지는 것도 크게 와닿지 않네요.  
> 그럼에도 화살표 함수를 사용하는 이유는 뭘까요?

그전에 먼저 자바스크립트에서의 this에 대해 알아보겠습니다.

---

### Javascript 함수의 this 바인딩

Javascript에서 this가 참조하는 것은 함수가 호출되는 방식에 따라 결정되는데, 이것을 "this 바인딩"이라고 합니다.

this 바인딩은 크게 4가지 규칙이 있습니다.

1. `기본 바인딩 (Default Binding)`

   기본 바인딩이란 아래의 규칙에 해당되지 않을 때 기본적으로 적용되는 바인딩 규칙입니다.  
   기본 바인딩이 적용될 경우 <u>this는 전역 객체</u>에 바인딩됩니다.  
   (브라우저 환경인 경우 window, Node.js 환경인 경우 global)

   ```js
   function foo() {
     const a = 10;
     console.log(this.a);
   }

   foo(); // undefined
   ```

   다음과 같은 함수 선언문에서 this는 전역 객체를 바인딩하고 전역 객체에 a가 없기 때문에 undefined가 출력됩니다.

2. `암시적 바인딩 (Implicit Binding)`

   암시적 바인딩이란 함수가 객체의 메서드로서 호출되는 상황에서 this가 바인딩되는 것을 말합니다.  
   이때 <u>this는 해당 함수를 호출한 객체</u>에 바인딩됩니다.

   즉, 메소드 호출 시 메소드 내부의 this는 해당 메소드를 호출한 객체에 바인딩됩니다.

   ```js
   const foo = {
     a: 20,
     bar: function () {
       console.log(this.a);
     },
   };

   foo.bar(); // 20
   ```

3. `명시적 바인딩 (Explicit Binding)`

   자바스크립트의 모든 함수는 <kbd>call()</kbd>, <kbd>apply()</kbd>, <kbd>bind()</kbd>라는 prototype 메서드를 가지고 있습니다.

   3가지 메서드 중 하나를 호출할 때 매개변수로 바인딩할 객체를 넘겨주면서 함수를 실행할 때의 컨텍스트 객체에 직접 바인딩 할 수 있습니다.

   ```js
   const foo = {
     a: 20,
   };

   function bar() {
     console.log(this.a);
   }

   bar.call(foo); // 20
   bar.apply(foo); // 20
   ```

   **_bind 메서드_**는 매개변수로 전달받은 객체로 this가 바인딩된 함수를 반환합니다. (하드 바인딩이라고도 합니다.)

   ```js
   const foo = {
     a: 20,
   };

   function bar() {
     console.log(this.a);
   }

   const bound = bar.bind(foo);

   bound(); // 20
   ```

4. `new Binding `

   자바스크립트의 <kbd>new 키워드</kbd>는 함수를 호출할 때 앞에 new 키워드를 사용하는 것으로 객체를 초기화할 때 사용합니다.  
   이때 사용되는 함수를 생성자 함수라고 하며, 생성자 함수는 대문자로 시작합니다.

   즉, 함수 앞에 <kbd>new</kbd>를 붙이면 return 값으로 객체를 반환합니다.

   여기서 생성자 함수에서는 <u>this는 생성자 함수로 생성할 객체</u>에 바인딩됩니다.

   ```js
   function Foo() {
     this.a = 20;
   }

   const foo = new Foo();

   console.log(foo.a); // 20
   ```

---

### 화살표 함수의 this 바인딩

ES6에 추가된 화살표 함수(Arrow Function)는 this를 바인딩할 때 앞서 설명한 규칙들이 적용되지 않습니다.

this에 Lexical scope가 적용되어 <u>화살표 함수를 정의하는 시점의 컨텍스트 객체</u>가 this에 바인딩된다.

쉽게 말하면 function으로 선언한 함수가 메소드로 호출되냐 함수 자체로 호출되냐에 따라 동적으로 this가 바인딩되는 반면, 화살표 함수는 `선언될 시점에서의 상위 스코프`가 this로 바인딩됩니다.

즉, this가 없기 때문에 상위 외부 함수에서 this를 가져옵니다.

이러한 특성 때문에 화살표 함수 사용에 몇몇의 제약이 생깁니다.

---

### 화살표 함수를 사용하면 안될 때

- `메소드 정의`  
  화살표 함수로 메소드를 정의하면 메소드를 호출한 객체가 아닌 상위 컨택스트인 전역 객체 window를 가르킵니다.

  ```js
  const foo = {
    tmp: "hello",
    log: () => console.log(`${this.tmp}`),
  };

  foo.log(); // undefined
  ```

- `prototype 메소드`  
  prototype에 할당할 경우에도 같은 문제가 발생합니다.

  ```js
  const foo = {
    tmp: "hello",
  };

  Object.prototype.log = () => console.log(`${this.tmp}`);

  foo.log(); // undefined
  ```

- `new 생성자 함수`  
  생성자 함수는 prototype 프로퍼티를 가지고 있으며 해당 프로퍼티가 가리키는 프로토타입 객체의 constructor를 사용합니다.

  화살표 함수는 prototype 프로퍼티를 가지고 있지 않습니다.  
  따라서, 화살표 함수를 생성자함수로 사용하면 에러가 납니다.

  ```js
  const Foo = (name) => {
    this.name = name;
  };

  // 화살표 함수는 prototype 프로퍼티가 없다
  console.log(Foo.hasOwnProperty("prototype")); // false

  const foo = new Foo("June"); // TypeError: Foo is not a constructor
  ```

- `addEventListener 함수의 콜백 함수`  
  addEventListener 함수의 콜백 함수를 화살표 함수로 정의하면 this가 상위 컨택스트인 전역 객체 window를 가리키게 됩니다.

  따라서, function 키워드로 정의한 일반 함수를 사용하여야 합니다.  
  (상위 스코프의 속성들을 사용하기위해 의도한 경우 사용할 수 있습니다.)

  ```js
  var button = document.getElementById("myButton");

  button.addEventListener("click", () => {
    console.log(this === window); // => true
    this.innerHTML = "Clicked button";
  });

  button.addEventListener("click", function () {
    console.log(this === button); // => true
    this.innerHTML = "Clicked button";
  });
  ```

- `call, apply, bind 메소드`  
  화살표 함수는 call, apply, bind 메소드를 사용하여 this를 변경할 수 없습니다.

  ```js
  window.x = 1;
  const normal = function () {
    return this.x;
  };
  const arrow = () => this.x;

  console.log(normal.call({ x: 10 })); // 10
  console.log(arrow.call({ x: 10 })); // 1
  ```

---

## 참조

- <https://velog.io/@takeknowledge/javscript-ES6%EC%97%90-%EC%B6%94%EA%B0%80%EB%90%9C-%EA%B8%B0%EB%8A%A5-%EA%B0%84%EB%8B%A8-%EC%A0%95%EB%A6%AC#2-%EA%B0%9D%EC%B2%B4-%EB%B9%84%EA%B5%AC%EC%A1%B0%ED%99%94--object-destructuring->
- <https://velog.io/@tkejt1343/Javascript-ES6%EC%97%90-%EC%B6%94%EA%B0%80%EB%90%9C-%EA%B2%83%EC%9D%80#arrow-function%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98>
- <https://velog.io/@padoling/JavaScript-%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98%EC%99%80-this-%EB%B0%94%EC%9D%B8%EB%94%A9>
- <https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98-%EC%A0%95%EB%A6%AC>
