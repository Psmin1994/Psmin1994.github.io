---
title: 타입 변환 Type casting
author: Psmin
data: 2023-07-07 22:17:33 +0900
categories: [Knowledge, Javascript]
tags: [Javascript]
---

## 타입 변환 Type casting

**타입 변환**이란 하나의 타입에서 다른 타입으로 바꾸는 것을 말합니다.

- **명시적 타입 변환**

  사용자가 의도해서 명시적으로 변경하는 것

- **암묵적 타입 변환**

  자바스크립트 엔진이 자동으로 변환하는 것

```js
let x = 10;

// 명시적 타입 변환
let str = x.toString(); // 숫자를 문자열로 변환

// 암묵적 타입 변환
let str = x + "";
```

---

{: .prompt-info}

> 타입별 명시적 타입 변환 방법에 대해 알아보자.

---

## String 타입

1. **new 연산자 없이 String 생성자 함수**
2. **Object.prototype.toString 메서드**
3. **문자열 연결 연산자**

```js
// 1. new 연산자 없이 String 생성자 함수
console.log(String(1)); // "1"
console.log(String(NaN)); // "NaN"
console.log(String(true)); // "true"
console.log(String(false)); // "false"

// 2. Object.prototype.toString 메서드
console.log((1).toString()); // "1"
console.log(NaN.toString()); // "NaN"
console.log(true.toString()); // "true"
console.log(false.toString()); // "false"

// 3. 문자열 연결 연산자
console.log(1 + ""); // "1"
console.log(NaN + ""); // "NaN"
console.log(true + ""); // "true"
console.log(false + ""); // "false"
```

---

## Number 타입

1. **new 연산자 없이 Number 생성자 함수**
2. **parseInt, parseFloat 함수** (문자열만 가능)
3. **+ 단항 연결 연산자**
4. **\* 산술 연산자**

```js
// 1. new 연산자 없이 Number 생성자 함수
console.log(Number("0")); // 0
console.log(Number("-1")); // -1
console.log(Number("10.53")); // 10.53
console.log(Number(true)); // 1
console.log(Number(false)); // 0

// 2. parseInt, parseFloat 함수 (문자열만 가능)
console.log(parseInt("0")); // 0
console.log(parseInt("-1")); // -1
console.log(parseFloat("10.53")); // 10.53

// 3. + 단항 연결 연산자를
console.log(+"0"); // 0
console.log(+"-1"); // -1
console.log(+"10.53"); // 10.53
console.log(+true); // 1
console.log(+false); // 0

// 4. * 산술 연산자를 이용하는 방법
console.log("0" * 1); // 0
console.log("-1" * 1); // -1
console.log("10.53" * 1); // 10.53
console.log(true * 1); // 1
console.log(false * 1); // 0
```

---

## Boolean 타입

1. **new 연산자 없이 Boolean 생성자 함수**
2. **!! 부정 논리 연산자**

```js
// 1. new 연산자 없이 Boolean 생성자 함수
console.log(Boolean("x")); // true
console.log(Boolean("")); // false
console.log(Boolean(0)); // false
console.log(Boolean(1)); // true
console.log(Boolean(NaN)); // false
console.log(Boolean(null)); // false
console.log(Boolean(undefined)); // false
console.log(Boolean({})); // true
console.log(Boolean([])); // true

// 2. !! 부정 논리 연산자
console.log(!!"x"); // true
console.log(!!""); // false
console.log(!!0); // false
console.log(!!1); // true
console.log(!!NaN); // false
console.log(!!null); // false
console.log(!!undefined); // false
console.log(!!{}); // true
console.log(!![]); // true
```

---

## 배열을 문자열로 변환

- **Array.prototype.join() 메서드**

```js
const numbers = [1, 2, 3];

console.log(numbers.join());
// "1,2,3"

console.log(numbers.join(""));
// "123"

console.log(numbers.join("-"));
// "1-2-3"
```

---

## 문자열을 배열로 변환

- **전개 연산자 Spread Operator (...)**
- **String.prototype.split() 메서드**
- **Array.from() 메서드**

```js
// 1. 전개 연산자 Spread Operator (...)
const str = "hello";

const arr = [...str];

console.log(Array.isArray(arr)); // true
console.log(arr); // [ 'h', 'e', 'l', 'l', 'o' ]

// 2. Array.from() 메서드
const str = "hello";

const arr = Array.from(str);

console.log(Array.isArray(arr)); // true
console.log(arr); // [ 'h', 'e', 'l', 'l', 'o' ]

// 3. String.prototype.split() 메서드
const str = "hello";

const arr = str.split("");

console.log(Array.isArray(arr)); // true
console.log(arr); // [ 'h', 'e', 'l', 'l', 'o' ]
```
