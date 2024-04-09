---
title: 프로토타입 prototype
author: Psmin
data: 2023-06-27 22:11:56 +0900
categories: [Knowledge, Javascript]
tags: [Prototype]
---

## 프로토타입

Java, C++과 같은 클래스 기반 객체지향 프로그래밍 언어와 달리 자바스크립트는 **프로토타입 기반 언어**입니다.

프로토타입은 객체 간의 **상속을 구현**하기 위해 사용됩니다.

**프로토타입 체인**을 통해 객체는 다른 객체의 속성과 메소드를 상속받을 수 있습니다.

<u>이는 코드의 재사용성을 높이고 메모리를 효율적으로 사용할 수 있게 도와줍니다.</u>

프로토타입은 객체를 확장하고 객체 지향적인 프로그래밍을 할 수 있도록 도와줍니다.

- **간단 예제**

  ```js
  let Person = (name) => {
    this.name = name;
  };

  Person.prototype.hello = (name) => {
    console.log(`hello, my name is ${this.name}`);
  };

  let joon = new Person("joon");

  joon.hello(); // joon
  ```

위 코드에서 **joon은 hello 메소드를 상속받아 사용**할 수 있습니다.

---

## 포로토타입 객체

ECMA-262에서 프로토타입 객체란 <u>다른 객체에 공유 프로퍼티(메서드 포함)를 제공하는 객체</u>를 말합니다.

> object that provides shared properties for other objects

자바스크립트의 모든 객체는 자신의 부모 역할을 담당하는 객체와 연결되어 있고, 부모 객체의 프로퍼티 또는 메소드를 상속받아 사용할 수 있습니다.

이러한 부모 객체를 프로토타입 객체라고 합니다.

모든 객체는 `[[Prototype]]`이라는 내부 슬롯(자바스크립트 엔진의 내부 로직)을 갖으며, 이는 상속을 구현하는 프로토타입 객체를 말합니다.

---

### 프로토타입 객체 접근

`[[Prototype]]` 내부 슬롯에는 직접 접근이 불가능하며 `__proto__` 프로퍼티로 접근할 수 있습니다.

`__proto__` 프로퍼티에 접근하면 내부적으로 `Object.getPrototypeOf`가 호출되어 프로토타입 객체를 반환합니다.

![prototype-obj](/assets/img/prototype-obj.png){: .normal}

```js
let joon = { name: "joon" };

console.log(joon.__proto__ === Object.prototype); // true
```

프로토타입 객체는 객체를 생성할 때 결정됩니다.

결정된 프로토타입 객체는 동적으로 변경할 수 있습니다.

```js
function Person(name) {
  this.name = name;
}

function foo() {
  this.x = 1;
}

let joon = new Person("joon");

console.log(joon.__proto__ === Person.prototype); // true

joon.__proto__ = foo.prototype;

console.log(joon.__proto__ === foo.prototype); // true
```

---

### Object.getPrototypeOf()를 사용하자.

`__proto__` 속성으로 직접 접근하는 것은 권장하지 않습니다.

ES6에서 표준으로 채택되었지만 모든 객체가 `__proto__`를 사용할 수 있는 것은 아닙니다.

따라서, 프로토타입에 접근할 때는 `Object.getPrototypeOf` 메서드를 사용할 것을 권장합니다.

프로토타입을 변경할 경우에는 `Object.setPrototypeOf` 메서드를 사용할 것을 권장합니다.

```js
function Person(name) {
  this.name = name;
}

function foo() {
  this.x = 1;
}

let joon = new Person("joon");

console.log(Object.getPrototypeOf(joon) === Person.prototype); // true

Object.setPrototypeOf(joon, foo.prototype);

console.log(Object.getPrototypeOf(joon) === foo.prototype); // true
```

{: .prompt-info}

> 프로토타입 객체를 변경하는 것은 객체의 상속 체계에 큰 영향을 줍니다.  
> 따라서 가급적 객체를 생성할 시점에 프로토타입을 설정해주는 것이 더 바람직합니다.

---

## 함수 객체의 prototype 프로퍼티

`prototype` 프로퍼티는 생성자 함수로 호출할 수 있는 객체, 즉 `constructor`를 소유하는 프로퍼티입니다.

함수 객체만 갖는 프로퍼티로 함수 객체가 생성자로 사용될 때 이 함수를 통해 생성될 객체의 부모 역할을 하는 프로토타입 객체를 가리킵니다.

```js
function Person() {}

var joon = new Person();

// Person() 생성자 함수에 의해 생성된 객체를 생성한 객체는 Person() 생성자 함수이다.
console.log(Person.prototype.constructor === Person);

// joon 객체를 생성한 객체는 Person() 생성자 함수이다.
console.log(Object.getPrototypeOf(joon).constructor === Person);
```

---

{: .prompt-tip}

> 여기서 중요한 것은 Person 함수의 prototype 속성이 참조하는 프로토타입 객체는 new라는 연산자와 Person 함수를 통해 생성된 모든 객체(인스턴스)의 원형이 되는 객체입니다.  
> 생성된 **모든 객체가 참조하는 것**을 기억합시다.

```js
function Person() {}

var joon = new Person();
var jisoo = new Person();
```

![prototype-01](/assets/img/prototype-01.png){: }

---

생성된 모든 객체는 프로토타입 객체에 접근할 수 있습니다.

즉, 같은 원형을 갖는 객체는 프로토타입 객체에 추가된 프로퍼티나 메서드를 사용할 수 있습니다.

추가되기 전 생성된 객체들도 사용할 수 있습니다.

```js
function Person() {}

var joon = new Person();

// 함수 객체의 prototype 프로퍼티를 사용해 메소드를 추가
Person.prototype.getType = function () {
  console.log("Hi");
};

joon.getType(); // Hi
```

![prototype-02](/assets/img/prototype-02.png){: }

---

## 오버라이딩

생성된 객체에서 프로토타입 객체의 프로퍼티를 수정할 수 있습니다.

다만, 프로토타입 객체에 영향을 주지는 않고 수정한 객체에 해당 프로퍼티가 **오버라이딩**되어 변경됩니다.

```js
function Person() {}

var jisoo = new Person();
var joon = new Person();

Person.prototype.getType = function () {
  console.log("Hi");
};

joon.getType = function () {
  console.log("Hi!!!!");
};

joon.getType(); // Hi!!!!
jisoo.getType(); // Hi

joon.age = 25;

console.log(joon.age); // 25
console.log(jisoo.age); // undefined
```

![prototype-03](/assets/img/prototype-03.png){: }

---

## 프로토타입 상속 예제

자바스크립트는 클래스가 없기 때문에 프로토타입을 이용하여 코드 재사용을 구현해보겠습니다.

- **기본 방법**

  부모에 해당하는 함수를 이용하여 객체를 생성합니다.

  자식에 해당하는 함수의 prototype 속성을 부모 함수를 이용하여 생성한 객체를 참조하는 방법입니다.

  kor2 객체를 생성할 때 Korean 함수의 인자로 '지수'라고 주었지만 부모 생성자에게 인자를 넘겨주지 않아 name에는 기본값인 혁준이 들어있습니다.

  ```js
  function Person(name) {
    this.name = "혁준";
  }

  Person.prototype.getName = function () {
    return this.name;
  };

  // 객체 생성자 함수
  function Korean(name) {}

  // 해당 생성자 함수의 .prototype 속성을 Person 객체로 설정
  Korean.prototype = new Person();

  var kor1 = new Korean();
  // Person의 프로토타입 객체에 선언된 메소드도 사용 가능
  console.log(kor1.getName()); // 혁준

  var kor2 = new Korean("지수");
  console.log(kor2.getName()); // 혁준
  ```

  ![prototype-05](/assets/img/prototype-05.png){: }

- **프로토타입 공유**

  부모 생성자를 호출하지 않으면서 프로토타입 객체를 공유하는 방법입니다.

  자식 함수의 .prototype 속성을 부모 함수의 .prototype 속성이 참조하는 객체로 설정합니다.

  자식 함수를 통해 생성된 객체는 부모 함수를 통해 생성된 객체를 거치지 않고 부모 함수의 프로토타입 객체를 부모로 지정하여 객체를 생성합니다.

  ```js
  function Person(name) {
    this.name = name || "혁준";
  }

  Person.prototype.getName = function () {
    return this.name;
  };

  // 객체 생성자 함수
  function Korean(name) {
    this.name = name;
  }

  // 자식 함수의 .prototype 속성을 부모 함수의 .prototype 속성이 참조하는 객체로 설정
  Korean.prototype = Person.prototype;

  var kor1 = new Korean("지수");
  console.log(kor1.getName()); // 지수
  ```

  ![prototype-06](/assets/img/prototype-06.png){: }

- **Object.create() 사용**

  `Object.create()` 를 사용해 객체를 생성과 동시에 프로토타입 객체를 지정합니다.

  첫 번째 매개변수는 부모 객체로 사용할 객체이고, 두 번째 매개변수는 반환될 자식 객체에 추가할 속성입니다.

  ```js
  var person = {
    type: "인간",
    getType: function () {
      return this.type;
    },
    getName: function () {
      return this.name;
    },
  };

  var joon = Object.create(person);

  joon.name = "혁준";

  console.log(joon.getType()); // 인간
  console.log(joon.getName()); // 혁준
  ```
