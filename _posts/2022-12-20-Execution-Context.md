---
title: ES6의 실행 컨텍스트 (Execution Context)
author: Psmin
data: 2022-12-20 03:16:45 +0900
categories: [Javascript]
tags: [Javascript]
---

# ES6의 Execution Context

---

## 실행 컨텍스트 `Execution Context`

코드가 실제로 실행되고 관리되는 영역을 말하며 실행 컨텍스트는 **3가지 경우**에 생성됩니다.

- 맨 처음 코드가 실행 되었을 때 -> `Global Execution Context`
- 함수가 호출되었을 때 -> `Functional Execution Context`
- eval()이 사용되었을 때

생성된 Execution Context는 **JS Engine**의 `Call Stack`에 쌓입니다.  
쌓인 컨텍스트는 **후입선출** 방식(Last In First Out)으로 현재의 컨텍스트가 완료되면 기존의 컨텍스트로 돌아가는 방식입니다.  
완료 되기 전 새로운 컨텍스트가 생성된다면 해당 컨텍스트가 **Call Stack**에 쌓이고 해당 컨텍스트를 먼저 완료 후 다시 진행하는 방식입니다.

![call-stack](/assets/img/js-call-stack.png){: .normal width="450"}

코드에 제공할 여러 환경 정보들을 **2가지의 컨포넌트**로 관리합니다.

- 렉시컬 환경 컴포넌트 `Lexical Environment`
- 변수 환경 컴포넌트 `Variable Environment`

---

### 렉시컬 환경 컴포넌트 `Lexical Environment`

Lexical Environment는 2가지의 구성요소로 이루어져있다.

![lexical-environment](/assets/img/lexical-environment.png){: .normal .w-75}

- 외부 렉시컬 환경에 대한 참조 `Outer Lexical Environment Reference`  
  자바스크립트는 함수 안에 함수를 중첩해서 정의할 수 있는 언어입니다.  
  유효 범위 너머의 유효 범위까지도 검색할 수 있어야합니다.  
  함수를 둘러싸고 있는 코드가 속한 **Lexical Environment** 컴포넌트의 참조가 저장됩니다.

  즉, 중첩된 함수 안에서 바깥 코드에 정의된 변수를 읽거나 사용할 때, `Outer Lexical Environment Reference`를 따라 한 단계씩 거슬러 올라가 변수를 검색합니다.

  ![outer-reference-environment](/assets/img/outer-reference-environment.png){: .normal .w-75}

- 환경 레코드 `Environment Records`  
  렉시컬 환경 안의 식별자와 식별자가 가리키는 값의 묶음이 저장되는 영역으로 2가지의 구성요소로 이루어져있다.

  - 선언적 환경 레코드 `Declarative Environment Records`  
    함수와 변수, this 등 식별자 바인딩 저장되는 영역입니다.
  - 객체 환경 레코드 `Object Environment Records`  
    실행 컨텍스트 외부에 별도로 저장된 객체의 참조에서 데이터를 읽거나 사용합니다.  
    with문의 Lexical Environment 또는 전역 객체처럼 별도의 객체에 저장된 데이터는 해당 객체 전체의 참조를 가져와서 객체 환경 레코드의 bindObject 프로퍼티에 바인드됩니다.

![lexical-environment-total](/assets/img/lexical-environment-total.png){: .normal .w-80}

---

### 변수 환경 컴포넌트 `Variable Environment`

렉시컬 환경 컴포넌트 타입과 구조가 같으며 이 둘의 차이점은 변수 선언 방식에 있습니다.  
위 사진을 자세히 보면 `Lexical Environmet`에는 `let`, `const`로 선언된 변수가 저장되며,  
`Variable Environment`에는 `var`로 선언된 변수가 저장됩니다.  
<br/>
이 둘의 차이점은 2가지 있습니다.

1. 변수의 `Scope` 차이  
   **let, const**로 선언된 변수는 `block scope`이고, **var**로 선언된 변수는 `function scope`입니다.  
   여기서 **scope 개념**을 가볍게 정의하면 **변수를 참조할 수 있는 유효 범위**입니다.

   즉, let, const로 선언된 변수는 `블록{중괄호 안}`을 기준으로 참조 유효 범위가 정해지고, var로 선언된 변수는 `함수`를 기준으로 참조 유효 범위가 정해집니다.  
   간단한 예제를 만들어 보겠습니다.

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

함수의 중괄호도 블록에 포함되기 때문에 test함수 내부의 x1,y1은 지역 변수로 외부에서 참조되지 않습니다.

```javascript
let x1 = "let-outer";
var y1 = "var-outer";

{
  let x1 = "let-inner";
  var y1 = "var-inner";
  console.log(x1); // let-inner
  console.log(y1); // var-inner
}

console.log(x1); // let-outer
console.log(y1); // var-inner
```

함수가 아닌 블록에는 var로 선언된 변수는 전역 변수이고, let으로 선언된 변수는 지역 변수입니다.  
따라서 var로 선언된 y1은 외부에서 참조가 가능해 var-inner가 결과 값으로 나타난 것을 확인할 수 있습니다.

2. 변수가 생성되는 과정  
   자바스크립트에서 변수가 생성돠는 과정은 3가지로 나누어집니다.

   1. **Declaration Phase** 선언 단계 : 변수를 Execution Context의 **Lexical Environment**에 등록합니다.
   2. **Initialization Phase** 초기화 단계 : Lexical Environment에 등록되어 있는 변수를 위한 **메모리가 할당**되고 `undefined`로 초기화됩니다.
   3. **Assignment Phase** 할당 단계 : 변수에 실제 값이 할당됩니다.  
      **var**은 1,2번이 동시에 진행됩니다.  
      즉, 평가 시점에 **전역 객체 (Binding Object)에 등록**되고, `undefined`을 갖습니다.

      **let, const**는 전역 객체의 프로퍼티가 아닌 블록 스코프 내에 존재합니다. (window.x로 참조 불가능)  
      평가를 마친 후 프로그램이 실행되어 초기화 단계(즉, 런타임이 실행되어 코드를 만나는 단계)까지 `TDZ`(**Temporal Dead Zone**)이 발생합니다.  
      선언 단계를 통해 환경 레코드에 등록되지만 undefined가 아닌 `uninitialize`를 갖습니다.  
      초기화 단계까지 사용 불가합니다.

   이러한 차이점이 있기 때문에 둘을 구분지어 저장합니다.
