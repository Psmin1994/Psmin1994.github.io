---
title: 프로그래머스 알고리즘 문제 - 05
author: Psmin
data: 2023-08-23 23:26:13 +0900
categories: [코딩테스트, Programmers]
tags: [programmers]
---

# Level 0

---

## 컨트롤 제트

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120853)

숫자와 "Z"가 공백으로 구분되어 담긴 문자열이 주어집니다.

문자열에 있는 숫자를 차례대로 더하려고 합니다.

이 때 "Z"가 나오면 바로 전에 더했던 숫자를 뺀다는 뜻입니다.

숫자와 "Z"로 이루어진 문자열 s가 주어질 때, 머쓱이가 구한 값을 return 하도록 solution 함수를 완성해보세요.

---

### 풀이

```js
function solution(s) {
  var arr = s.split(" ");
  let stack = [];

  arr.forEach((x) => {
    x == "Z" ? stack.pop() : stack.push(x);
  });

  return stack.reduce((acc, value) => acc + Number(value), 0);
}
```

---

## 영어가 싫어요.

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120894)

영어가 싫은 머쓱이는 영어로 표기되어있는 숫자를 수로 바꾸려고 합니다.

문자열 numbers가 매개변수로 주어질 때, numbers를 정수로 바꿔 return 하도록 solution 함수를 완성해 주세요.

---

### 풀이

- `String.prototype.replaceAll()`

  패턴이 일치하는 모든 부분이 교체된 새로운 문자열을 반환합니다.

  > str.replaceAll(regexp|substr, newSubstr|function)

  1. 첫 번째 인자

     - **regexp** : 정규식(RegExp) 객체 또는 리터럴
     - **substr** : newSubStr로 변경 될 문자열

  2. 두 번째 인자

     - **newSubStr** : 첫 번째 인자를 대신해 변경되는 문자열
     - **function** : 첫 번째 인자를 대신해 변경되는 새 하위 문자열을 생성하기 위해 호출되는 함수.

```js
function solution(numbers) {
  let nums = [
    "zero",
    "one",
    "two",
    "three",
    "four",
    "five",
    "six",
    "seven",
    "eight",
    "nine",
  ];

  let map = new Map();
  let answer = 0;

  nums.forEach((x, i) => map.set(x, i));

  for (let [key, num] of map) {
    numbers = numbers.replaceAll(key, num);
  }

  return Number(numbers);
}
```

---

## 잘라서 배열로 저장하기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120913)

문자열 my_str과 n이 매개변수로 주어질 때, my_str을 길이 n씩 잘라서 저장한 배열을 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- **RegExp 생성자**

  패턴을 사용해 텍스트를 판별할 때 사용하는 정규 표현식 객체를 생성합니다.

  > /pattern/flags
  > new RegExp(pattern[, flags])
  > RegExp(pattern[, flags])

  - **pattern** : 정규 표현식을 나타내는 문자열
  - **flags** : 정규 표현식에 추가할 플래그
    - g : 전체 판별
    - i : 대소문자 무시
    - m : 여러 줄

> 추가로 정리 필요 !

```js
function solution(my_str, n) {
  return my_str.match(new RegExp(`.{1,${n}}`, "g"));
}
```

---

## 등수 매기기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120882)

영어 점수와 수학 점수의 평균 점수를 기준으로 학생들의 등수를 매기려고 합니다.

영어 점수와 수학 점수를 담은 2차원 정수 배열 score가 주어질 때, 영어 점수와 수학 점수의 평균을 기준으로 매긴 등수를 담은 배열을 return하도록 solution 함수를 완성해주세요.

---

### 풀이

등수나 순위 등 비교할 배열을 `avg_score.sort()` 하면 원본 배열도 정렬되는 것을 기억하자.

```js
function solution(score) {
  var avg_score = score.map((el) => {
    let [math, eng] = el;

    return (math + eng) / 2;
  });

  var sorted_avg = [...avg_score].sort((a, b) => b - a);

  return avg_score.map((el) => sorted_avg.indexOf(el) + 1);
}
```

---

## 문자열 밀기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120921)

문자열 "hello"에서 각 문자를 오른쪽으로 한 칸씩 밀고 마지막 문자는 맨 앞으로 이동시키면 "ohell"이 됩니다.

이것을 문자열을 민다고 정의한다면 문자열 A와 B가 매개변수로 주어질 때, A를 밀어서 B가 될 수 있다면 밀어야 하는 최소 횟수를 return하고 밀어서 B가 될 수 없으면 -1을 return 하도록 solution 함수를 완성해보세요.

---

### 풀이

```js
function solution(A, B) {
  if (A === B) return 0;

  for (let i = 0; i < A.length; i++) {
    A = A.slice(-1) + A.slice(0, -1);
    if (A === B) return i + 1;
  }
  return -1;
}
```

```js
// 아이디어 방법 - CSS 스프라이트 느낌
let solution = (a, b) => (b + b).indexOf(a);
```

---

## 다항식 더하기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120863)

한 개 이상의 항의 합으로 이루어진 식을 다항식이라고 합니다. 다항식을 계산할 때는 동류항끼리 계산해 정리합니다.

덧셈으로 이루어진 다항식 polynomial이 매개변수로 주어질 때, 동류항끼리 더한 결괏값을 문자열로 return 하도록 solution 함수를 완성해보세요. 같은 식이라면 가장 짧은 수식을 return 합니다.

---

### 풀이

- 인자가 문자열에 없을 때 split()의 반환값은 요청한 문자열 하나를 담은 배열
- replace() 결과가 없을 때 || 로 값 주기
- filter 활용 생각하기
- 배열에 담아 join()으로 문자열 완성

```js
function solution(polynomial) {
  var arr = polynomial.split(" + ");

  var xNum = arr
    .filter((el) => el.includes("x"))
    .map((el) => {
      return el.replace("x", "") || 1;
    })
    .reduce((acc, value) => {
      return acc + Number(value);
    }, 0);

  var num = arr
    .filter((el) => !el.includes("x"))
    .reduce((acc, value) => {
      return acc + Number(value);
    }, 0);

  var answer = [];

  if (xNum) answer.push(`${xNum == 1 ? "" : xNum}x`);
  if (num) answer.push(`${num}`);

  return answer.join(" + ");
}
```
