---
title: 프로그래머스 알고리즘 문제 - 02
author: Psmin
data: 2023-08-08 21:34:54 +0900
categories: [코딩테스트, Programmers]
tags: [programmers]
---

# Level 0

---

## 문자열 뒤집기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120822)

문자열 my_string이 매개변수로 주어집니다. my_string을 거꾸로 뒤집은 문자열을 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- `String.prototype.split()`

  split() 메서드는 String 객체를 **지정한 구분자를 기준으로 나눈 배열**을 반환합니다.

  > str.split(separator, limit)

  - **separator** : 원본 문자열을 끊어야 할 부분
  - **limit** : separator가 나올때마다 문자열을 끊어 배열에 넣을 갯수 제한

- `Array.prototype.reverse()`

  reverse() 메서드는 **배열의 순서를 반전**합니다.

- `Array.prototype.join()`

  join() 메서드는 **배열의 모든 요소를 연결해 하나의 문자열**로 만듭니다.

  > arr.join([separator])

  - **separator** : 배열의 각 요소를 구분할 문자열

```js
//solution.js

function solution(my_string) {
  var arr = my_string.split("");

  return arr.reverse().join("");
}
```

---

## 옷 가게 할인 받기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120818)

머쓱이네 옷가게는 10만 원 이상 사면 5%, 30만 원 이상 사면 10%, 50만 원 이상 사면 20%를 할인해줍니다.  
구매한 옷의 가격 price가 주어질 때, 지불해야 할 금액을 return 하도록 solution 함수를 완성해보세요.  
(소수점 이하를 버린 정수를 return합니다.)

---

### 풀이

- **tilde(~) 연산자**

  비트연산자로 0과 1을 뒤바꿔줍니다. (NOT 기능)

  ```js
  const a = 5; // 0000000000000101
  console.log(~a); // 1111111111111010
  // -6
  ```

- **double tilde(~~) 연산자**

  tilde를 2번 반복해서 다시 원래의 값으로 돌아옵니다.

  ~ 연산을 진행하면 `Math.floor()` 메서드를 사용한 것처럼 소수점들이 버려집니다.

  {: .prompt-info}

  > ~~, Math.floor, parseInt 순서로 속도가 빠릅니다.  
  > 하지만 가독성이 좋지 않으니 남용하지말자.

```js
function solution(price) {
  return ~~(price / 100000 >= 5
    ? price * 0.8
    : price / 100000 >= 3
    ? price * 0.9
    : price / 100000 >= 1
    ? price * 0.95
    : price);
}
```

---

## 배열의 유사도

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120903)

두 배열이 얼마나 유사한지 확인해보려고 합니다. 문자열 배열 s1과 s2가 주어질 때 같은 원소의 개수를 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- `Array.prototype.includes`

  배열이 특정 요소를 포함하고 있는지 판별합니다.

  > arr.includes(valueToFind[, fromIndex])

  - **ValueToFind** : 탐색할 요소 (대소문자 구분)
  - **fromIndex** : 검색을 시작할 인덱스 값

```js
function solution(s1, s2) {
  var answer = 0;

  s1.map((x) => {
    s2.includes(x) && answer++;
  });

  return answer;
}
```

---

## 문자 반복 출력하기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120825)

문자열 my_string과 정수 n이 매개변수로 주어질 때, my_string에 들어있는 각 문자를 n만큼 반복한 문자열을 return 하도록 solution 함수를 완성해보세요.

---

### 풀이

- **Spread Operator (...)**

  반복 가능한(iterable) 객체에 적용할 수 있는 문법입니다.

  배열이나 문자열 등을 풀어서 요소 하나 하나로 전개시킬 수 있습니다.

```js
function solution(my_string, n) {
  var answer = my_string;

  answer = [...my_string].map((x) => x.repeat(n));

  return answer.join("");
}
```

---

## 외계행성의 나이

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120834)

우주여행을 하던 머쓱이는 엔진 고장으로 PROGRAMMERS-962 행성에 불시착하게 됐습니다.  
입국심사에서 나이를 말해야 하는데, PROGRAMMERS-962 행성에서는 나이를 알파벳으로 말하고 있습니다.  
a는 0, b는 1, c는 2, ..., j는 9입니다. 예를 들어 23살은 cd, 51살은 fb로 표현합니다.  
나이 age가 매개변수로 주어질 때 PROGRAMMER-962식 나이를 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- `String.prototype.charCodeAt()`

  charCodeAt() 메서드는 주어진 인덱스에 대한 UTF-16 코드를 나타내는 0부터 65535 사이의 정수를 반환합니다.

  > str.charCodeAt(index)

  - **index** : 0 이상이며 문자열의 길이보다 작은 정수, 문자열에서 변환할 인덱스 값

- `String.prototype.fromCharCode()`

  String.fromCharCode() 메서드는 UTF-16 코드 유닛의 시퀀스로부터 문자열을 생성해 반환합니다.

  > String.fromCharCode(num1[, ...[, numN]])

  - **num1, ..., numN** : UTF-16 코드 유닛인 숫자 뭉치

- **ASCII 아스키코드**

  ![ASCII](/assets/img/ascii.png){: .normal}

```js
function solution(age) {
  var arr = [...age.toString()];

  let answer = arr.map((x) =>
    String.fromCharCode("a".charCodeAt(0) + Number(x))
  );

  return answer.join("");
}
```
