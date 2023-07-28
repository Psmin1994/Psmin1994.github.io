---
title: Prototype과 __proto__
author: Psmin
data: 2023-03-31 22:11:56 +0900
categories: [Knowledge, Javascript]
tags: [Prototype]
---

## 프로토타입 기반 언어

Java, C++과 같은 클래스 기반 객체지향 프로그래밍 언어와 달리 자바스크립트는 ES6에서 Class(이하 클래스)가 도입되었지만 프로토타입 기반 언어입니다.

객체 원형인 프로토타입을 이용해 새로운 객체를 생성합니다.  
생성된 객체는 또 다른 객체의 원형이 될 수 있습니다.

프로토타입은 객체를 확장하고 객체 지향적인 프로그래밍을 할 수 있도록 도와줍니다.

따라서 자바스크립트의 동작 원리를 이해하기 위해서는 프로토타입의 개념을 잘 이해하고 있어야 합니다.

프로토타입은 크게 2가지가 있습니다.

먼저 프로토타입 객체를 참조하는 `prototype 속성`이 있고, 객체 멤버인 proto 속성이 참조하는 숨은 링크가 있습니다.

둘의 차이점을 이해하기 위해 함수와 객체 내부 구조부터 알아보겠습니다.

---

### 함수와 객체의 내부 구조

예제와 함께 살펴보겠습니다.

```js
function Person() {}
```

속성이 없는 Person 함수를 정의했습니다. 파싱 단계에 들어가면, Person 함수 Prototype 속성은 프로토타입 객체를 참조합니다.

프로토타입 객체 멤버인 constructor 속성은 Person 함수를 참조하는 구조를 갖습니다.

{: .prompt-tip}

> 여기서 중요한 것은 Person 함수의 prototype 속성이 참조하는 프로토타입 객체는 new라는 연산자와 Person 함수를 통해 생성된 모든 객체(인스턴스)의 원형이 되는 객체입니다.  
> 생성된 모든 객체가 참조하는 것을 기억합시다.

```js
function Person() {}

var joon = new Person();
var jisoo = new Person();
```

![prototype-01](/assets/img/prototype-01.png){: .w-80}

---

## 프로토타입 객체

프로토타입 객체란 함수를 정의하면 다른 곳에 생성되는 다른 객체의 원형이 되는 객체입니다.

생성된 모든 객체는 프로토타입 객체에 접근할 수 있습니다.

즉, 같은 원형을 갖는 객체는 프로토타입 객체에 추가된 프로퍼티나 메서드를 사용할 수 있습니다.

추가되기 전 생성된 객체들도 사용할 수 있습니다.

```js
function Person() {}

var joon = new Person();

Person.prototype.getType = function () {
  console.log("Hi");
};

joon.getType(); // Hi
```

![prototype-02](/assets/img/prototype-02.png){: .w-80}

---

생성된 객체에서 프로토타입 객체의 프로퍼티를 수정할 수 있습니다. 다만, 프로토타입 객체에 영향을 주지는 않고 수정한 객체에 해당 프로퍼티가 오버라이딩되어 변경됩니다.

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

![prototype-03](/assets/img/prototype-03.png){: .w-80}

---

## 프로토타입

객체는 자신을 만드는데 사용된 원형인 프로토타입 객체를 이용해서 만듭니다.

원형을 바탕으로 생성된 객체 안에는 `__proto__ (비표준)` 속성이 원형 객체인 프로토타입 객체를 참조합니다.

여기서 `__proto__` 를 프로토타입 이라고 정의합니다.

![prototype-04](/assets/img/prototype-04.png){: .w-80}

{: .prompt_tip}

> 함수의 멤버인 .prototype 속성은 프로토타입 객체를 참조
> `__proto__` 는 자신을 만들어낸 원형인 프로토타입 객체를 참조

---

## 코드의 재사용 (상속?)

자바스크립트는 클래스가 없기 때문에 프로토타입을 이용하여 코드 재사용을 구현해보겠습니다.

- 기본 방법
  : 부모에 해당하는 함수를 이용하여 객체를 생성합니다.  
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
  // Person의 프로토타입 객체에 선언된 메소드도 사용 가능
  console.log(kor2.getName()); // 혁준
  ```

  ![prototype-05](/assets/img/prototype-05.png){: .w-80}

- 프로토타입 공유
  : 부모 생성자를 호출하지 않으면서 프로토타입 객체를 공유하는 방법입니다.

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

  ![prototype-06](/assets/img/prototype-06.png){: .w-80}

- Object.create() 사용
  : `Object.create()` 를 사용해 객체를 생성과 동시에 프로토타입 객체를 지정합니다.

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
