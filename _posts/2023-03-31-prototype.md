---
title: [[Prototype]]과 __proto__
author: Psmin
data: 2023-03-31 22:11:56 +0900
categories: [Knowledge, Javascript]
tags: [Prototype]
---

## 프로토타입 (Prototype)

Java, C++과 같은 클래스 기반 객체지향 프로그래밍 언어와 달리 자바스크립트는 프로토타입 기반 객체지향 프로그래밍 언어입니다.
따라서 자바스크립트의 동작 원리를 이해하기 위해서는 프로토타입의 개념을 잘 이해하고 있어야 합니다.

자바스크립트는 ES6에서 Class(이하 클래스)가 도입되었지만 프로토타입 기반 언어이므로, 다른 언어에서 사용되는 클래스와 동작 방식이 약간 다릅니다.

자바스크립트의 모든 객체는 자신의 부모 역할을 담당하는 객체와 연결되어 있습니다.  
이것은 마치 객체 지향의 상속 개념과 같이 부모 객체의 프로퍼티 또는 메소드를 상속받아 사용할 수 있게 해줍니다.  
이러한 부모 객체를 **_Prototype(프로토타입) 객체_** 또는 줄여서 **_Prototype(프로토타입)_**이라 한다.

```js
var student = {
  name: "Lee",
  score: 90,
};

// student에는 hasOwnProperty 메소드가 없지만 아래 구문은 동작한다.
console.log(student.hasOwnProperty("name")); // true
```

![prototype-ex](/assets/img/prototype-ex.png){: .w-80}

[[Prototype]]의 값은 Prototype(프로토타입) 객체이며 \_\_proto\_\_ 로 접근할 수 있습니다.  
\_\_proto\_\_ 프로퍼티에 접근하면 내부적으로 `Object.getPrototypeOf`가 호출되어 프로토타입 객체를 반환하게됩니다.

student 객체는 **proto** 프로퍼티로 자신의 부모 객체(프로토타입 객체)인 Object.prototype을 가리키고 있습니다.

```js
console.log(student.__proto__ === Object.prototype); // true
```

객체를 생성할 때 프로토타입은 결정됩니다.  
결정된 프로토타입 객체는 다른 임의의 객체로 변경할 수 있습니다.  
이것은 부모 객체인 프로토타입을 동적으로 변경할 수 있다는 것을 활용하여 객체의 상속을 구현할 수 있습니다.

---

## 속성 추가

자바와 같은 대부분의 언어에서는 클래스의 메소드가 클래스와 동시에 정의되지만 자바스크립트에서는 함수를 Class의 자바스크립트 Object 속성으로 추가합니다.

`this.functionName = function() {}` 을 사용해 함수를 추가합니다.

```js
function ExampleClass() {
  this.name = "Javascript";
  // 함수 추가
  this.sayName = function () {
    console.log(this.name);
  };
}

// 객체 인스턴스 생성
let object1 = new ExampleClass();
object1.sayName(); // Javascript
```

생성자에서 sayName 함수를 동적으로 추가합니다. 이를 **_프로토타입 활용 상속_**이라고 합니다.

- .prototype 속성을 사용해 해당 객체의 Object 속성을 동적으로 확장할 수 있습니다.

```js
function ExampleClass() {
  this.array = [1, 2, 3, 4];
  this.name = "Javascript";
}

let object = new ExampleClass();

Example.prototype.sayName = function () {
  console.log(this.name);
};

object.sayName(); // Javascript
```

즉, 생성자에서 추가하거나 .prototype 속성을 통해 추가할 수 있습니다.

> 둘의 차이는 무엇일까요?

생성자에서 추가된 경우는 생성된 객체의 인스턴스는 공통 프로퍼티이지만 각각 해당 프로퍼티를 갖고 있어 메모리가 낭비됩니다.

이 때, .prototype 속성으로 구현하여, 인스턴스들이 하나의 프로퍼티를 사용하여 중복을 제거할 수 있습니다.
