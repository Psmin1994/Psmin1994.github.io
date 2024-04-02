---
title: 실행 컨텍스트 Execution Context
author: Psmin
data: 2022-12-17 03:16:45 +0900
categories: [Knowledge, Javascript]
tags: [Javascript]
---

# ES6의 Execution Context

---

## 실행 컨텍스트 `Execution Context`

코드가 실제로 실행되고 관리되는 영역을 말하며 실행 컨텍스트는 **3가지 경우**에 생성됩니다.

- 맨 처음 코드가 실행 되었을 때 => `Global Execution Context`
- 함수가 호출되었을 때 => `Functional Execution Context`
- eval()이 사용되었을 때

생성된 Execution Context는 **_JS Engine_**의 `Call Stack`에 쌓입니다.

쌓인 컨텍스트는 **_후입선출 방식_**(Last In First Out)으로 현재의 컨텍스트가 완료되면 기존의 컨텍스트로 돌아가는 방식입니다.

완료 되기 전 새로운 컨텍스트가 생성된다면 해당 컨텍스트가 **_Call Stack_**에 쌓이고 해당 컨텍스트를 먼저 완료 후 다시 진행하는 방식입니다.

![call-stack](/assets/img/js-call-stack.png){: .normal width="450"}

---

### 실행 컨텍스트 구성

코드에 제공할 여러 환경 정보들을 `2개의 Lexical Environment`로 관리합니다.

이렇게 들어간 두 개의 Lexical Environment는 각각 다른 이름으로 구별되어 불린다.

- **_렉시컬 환경 컴포넌트_** `Lexical Environment`
- **_변수 환경 컴포넌트_** `Variable Environment`

두 환경 컴포넌트는 타입과 구조가 같으며 이 둘의 차이점은 변수 선언 방식에 있습니다.

{: .propmt-info}

> 변수의 Scope 차이

**let, const**로 선언된 변수는 `block scope`이고, **var**로 선언된 변수는 `function scope`입니다.

여기서 **scope 개념**을 가볍게 정의하면 **변수를 참조할 수 있는 유효 범위**입니다.

즉, let, const로 선언된 변수는 `블록{중괄호 안}`을 기준으로 참조 유효 범위가 정해지고, var로 선언된 변수는 `함수`를 기준으로 참조 유효 범위가 정해집니다.

- `간단 예제`

  ```javascript
  let x1 = "let-outer";
  var y1 = "var-outer";

  function test() {
    let x1 = "let-inner";
    var y1 = "var-inner";
    console.log(x1);
    console.log(y1);
  }

  test(); // let-inner, var-inner
  console.log(x1); // let-outer
  console.log(y1); // var-outer
  ```

---

## 렉시컬 컴포넌트 `Lexical Environment`

Lexical Environment는 2가지의 구성요소로 이루어져있습니다.

![lexical-environment](/assets/img/lexical-environment.png){: .normal }

---

### 외부 렉시컬 환경에 대한 참조 `Outer Lexical Environment Reference`

자바스크립트는 함수 안에 함수를 중첩해서 정의할 수 있는 언어입니다.

따라서, 유효 범위 너머의 유효 범위까지도 검색할 수 있어야합니다.

함수를 둘러싸고 있는 코드가 속한 **Lexical Environment**의 참조가 저장됩니다.

즉, 중첩된 함수 안에서 사용할 변수가 없다면, `Outer Lexical Environment Reference`를 따라 한 단계씩 거슬러 올라가 바깥 코드에 정의된 변수를 찾아 사용합니다.

자바스크립트에서 스코프 결정 시 렉시컬 스코프(정적인 스코프)를 따르므로 외부 환경 참조 값의 결정은 함수가 호출된 위치가 아닌 <u>함수가 선언된 위치에 따라 결정</u>된다.

![outer-reference-environment](/assets/img/outer-reference-environment.png){: .normal }

---

### 환경 레코드 `Environment Records`

식별자와 식별자가 가리키는 값의 묶음이 저장되는 영역으로 2가지의 구성요소로 이루어져있다.

- **_선언적 환경 레코드_** `Declarative Environment Records`

  함수와 변수, this 등 식별자 바인딩 저장영역입니다.

- **_객체 환경 레코드_** `Object Environment Records`

  실행 컨텍스트 외부에 별도로 저장된 객체 (with문) 의 참조에서 데이터를 읽거나 사용합니다.

  with문의 Lexical Environment 또는 전역 객체처럼 별도의 객체에 저장된 데이터는 해당 객체 전체의 참조를 가져와서 객체 환경 레코드의 bindObject 프로퍼티에 바인드됩니다.

{: .prompt-info}

> Variable Environment 와 Lexical Environment 는 각각 다른 방식으로 선언된 변수들을 관리합니다.

`Variable Environment` 에는 `var` 로 선언된 변수가 메모리에 매핑되며 초기값으로 undefined 가 할당됩니다.

변수 값 할당 코드가 실행되기 전 변수에 접근하게 되면 undefined 값을 얻게 되며, 할당 코드가 실행되고 난 뒤에는 해당 값으로 수정됩니다.

선언형 함수가 메모리에 매핑되며 함수 전체가 메모리에 할당된다.

`Lexical Environment` 에는 `let`, `const` 로 선언된 변수가 메모리에 매핑되지만 초기값은 할당되지 않습니다.

변수 값 할당 코드가 실행되기 전 변수에 접근하게 되면 `reference error`가 발생합니다.

초기값 할당 코드가 실행되고 난 뒤에 메모리에 값이 추가됩니다.

---

### TDZ (Temporal Dead Zone)

자바스크립트에서 변수가 생성돠는 과정은 3가지로 나누어집니다.

1.  **_선언 단계_** `Declaration Phase`  
    변수를 Execution Context의 **Lexical Environment**에 등록합니다.

2.  **_초기화 단계_** `Initialization Phase`  
    Lexical Environment에 등록되어 있는 변수를 위한 **메모리가 할당**되고 `undefined`로 초기화됩니다.

3.  **_선언 단계_** `Assignment Phase`  
    변수에 실제 값이 할당됩니다.

**var**은 1,2번이 동시에 진행됩니다.

즉, 평가 시점에 **전역 객체 (Binding Object)에 등록**되고, `undefined`을 갖습니다.

**let, const**는 전역 객체의 프로퍼티가 아닌 블록 스코프 내에 존재합니다. (window.x로 참조 불가능)

평가를 마친 후 프로그램이 실행되어 초기화 단계(즉, 런타임이 실행되어 코드를 만나는 단계)까지 `TDZ`(**Temporal Dead Zone**)이 발생합니다.

선언 단계를 통해 환경 레코드에 등록되지만 undefined가 아닌 `uninitialize`를 갖습니다.  
 초기화 단계까지 사용 불가합니다.

이러한 차이점이 있기 때문에 둘을 구분지어 저장합니다.

---

## 실행 컨텍스트 생성 과정

실행 컨텍스트는 `Creation` 과 `Execution` 의 두 단계를 거쳐 생성됩니다.

- **_생성 단계_** `Creation Phase`

  **_Lexical Environment_** 와 **_Variable Environment_** 의 정의가 이루어집니다.

  this 바인딩과 외부 환경 참조 값 `Outer Lexical Environment Reference`을 결정합니다.

  `Environment Record` 에 변수 식별자에 대한 메모리가 매핑되며 값의 할당은 선언 방식에 따라 다르게 이루어집니다.

  먼저 `Lexical Environment` 에는 `let`, `const` 로 선언된 변수가 메모리에 매핑되지만 코드 실행 시점에 실제 코드 선언부를 만나기 전까지 초기값은 할당되지 않습니다.

  `Variable Environment` 에는 `var` 로 선언된 변수가 메모리에 매핑되며 초기값으로 `undefined` 가 할당됩니다.

  또한 선언형 함수가 메모리에 매핑되며 함수 전체가 메모리에 할당됩니다.

- **_실행단계_** `Execution Phase`

  생성 단계에서 코드 실행을 위한 환경 정보 값이 결정되었다면 코드를 위에서부터 읽으며 실행합니다.

  변수 값이 할당되는 코드가 실행될 경우 `Environment Record` 에 저장된 식별자 메모리에 값을 수정 또는 할당합니다.

---

## 실행 컨텍스트 생성 과정 예제

```js
function sayHelloOneTime() {
  var isMorning = true;
  let hello = "Good morning!";

  while (isMorning) {
    var name = "Ron";
    let question = "How are you?";

    console.log(`${name} ${hello} ${question}`);
    isMorning = false;
  }
}

sayHelloOneTime();
```

![execution-context.png](/assets/img/execution-context.png){: .normal }

`sayHelloOneTime` 가 호출될 때 `Execution Context` 를 생성합니다.

`Variable Environment` 는 `var` 가 유효한 **_함수 스코프를 범위_**로 가지고 있기 때문에 sayHelloOneTime 의 스코프 내에 있는 var 로 선언된 `isMorning` 과 `name` 의 식별자 정보를 매핑합니다.

`Lexical Environment` 는 `let`, `const` 가 유효한 **_블록 스코프를 범위_**로 가지고 있기 때문에 시작은 `Variable Environment` 와 같이 sayHelloOneTime 의 블록을 범위로 가집니다.

또 `Environment Record` 에 `let` 으로 선언된 hello 의 정보를 매핑합니다.

그리고 `sayHelloOneTime` 에는 `while 블록`이 있습니다.

while 블록은 다시 `Lexical Environment` 를 정의하며, `Environment Record` 에 `let` 으로 선언된 `question` 의 정보를 매핑합니다.

또 외부 환경 참조 값으로 `sayHelloOneTime` 의 `Lexical Environment` 를 갖습니다.
