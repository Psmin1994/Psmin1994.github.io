---
title: Template Literal (템플릿 리터럴)
author: Psmin
data: 2022-12-24 14:21:23 +0900
categories: [Nodejs]
tags: [Template Literal, ES6]
---

# 템플릿 리터럴에 대해 알아보자.

- 추가로 ES6에서 추가된 기능들을 간단하게 정리해보자.

---

## 템플릿 리터럴 Template Literal

템플릿 리터럴란 내장된 표현식을 허용하는 문자열 리터럴입니다.  
여러 줄로 이뤄진 문자열과 문자 보간기능을 사용할 수 있습니다.

백틱(` `) 을 사용해 표현합니다.  
또한, 플레이스 홀더를 이용하여 표현식($ {expression})을 넣을 수 있습니다.  
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

1. 기본 바인딩 (Default Binding)  
   기본 바인딩이란 아래의 규칙에 해당되지 않을 때 기본적으로 적용되는 바인딩 규칙입니다.  
   기본 바인딩이 적용될 경우 this는 전역 객체에 바인딩됩니다.  
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
   이때 this는 해당 함수를 호출한 객체에 바인딩됩니다.

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

```

```
