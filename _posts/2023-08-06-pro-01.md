---
title: 프로그래머스 알고리즘 문제 - 01
author: Psmin
data: 2023-08-06 13:24:54 +0900
categories: [코딩테스트, Programmers]
tags: [programmers]
---

# Level 0

---

## 몪 구하기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120805)

정수 num1, num2가 매개변수로 주어질 때, num1을 num2로 나눈 몫을 return 하도록 solution 함수를 완성해주세요.

---

### 풀이

`parseInt` 와 `Number` 둘다 문자열을 숫자로 변환하는 함수입니다.

- `parseInt()`는 정수만 출력, `Number()`는 소수점 17자리까지 출력합니다.
- `Number()`는 오직 숫자 타입만 변환합니다.

```js
// solution.js

function solution(num1, num2) {
  return parseInt(num1 / num2);
}
```

---

## 숫자 비교하기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120807)

정수 num1과 num2가 매개변수로 주어집니다.

두 수가 같으면 1 다르면 -1을 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- **삼항 연산자**

  > condition ? exprIfTrue : exprIfFalse

  조건문 **?** 조건문이 참일 경우 실행할 표현식 **:** 조건문이 거짓일 경우 실행할 표현식

```js
// solution.js

function solution(num1, num2) {
  return num1 == num2 ? 1 : -1;
}
```

---

## 배열 두배 만들기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120809)

정수 배열 numbers가 매개변수로 주어집니다.

numbers의 각 원소에 두배한 원소를 가진 배열을 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- `Array.prototype.map()`

  배열 내의 모든 요소 각각에 대하여 주어진 함수를 호출한 결과를 모아 새로운 배열을 반환합니다.

  > arr.map(function(element, index, array){ }, this);

  - **element** : 현재 요소 값
  - **index** : 현재 요소의 인덱스 값
  - **array** : map()을 호출한 배열
  - **this** : callback 에서의 `this` 값

```js
//solution.js

function solution(numbers) {
  return numbers.map((x) => x * 2);
}
```

---

## 중앙값 구하기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120811)

중앙값은 어떤 주어진 값들을 크기의 순서대로 정렬했을 때 가장 중앙에 위치하는 값을 의미합니다.

예를 들어 1, 2, 7, 10, 11의 중앙값은 7입니다.

정수 배열 array가 매개변수로 주어질 때, 중앙값을 return 하도록 solution 함수를 완성해보세요.

### 풀이

- `Array.prototype.sort()`

  배열의 요소를 비교 함수에 따라 정렬한 후 반환합니다.

  **원 배열이 정렬되는 것**을 기억하자.

  > arr.sort([compareFunction])

  - **compareFunction** : 정렬 순서를 정의하는 함수

    생략 시, 문자열로 취급하고 유니코드 값 순서로 정렬합니다.

    `compareFunction(a, b)` 사용 시, 함수의 반환 값이 0 보다 작으면 a, b 순서로 0 이면 변경없이 0 보다 크면 b, a 순서로 정렬합니다.

```js
// solution.js

function solution(array) {
  const index = parseInt(array.length / 2);

  array.sort((a, b) => {
    return a - b;
  });

  return array[index];
}
```

---

## 최빈값 구하기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120812)

최빈값은 주어진 값 중에서 가장 자주 나오는 값을 의미합니다.

정수 배열 array가 매개변수로 주어질 때, 최빈값을 return 하도록 solution 함수를 완성해보세요.

최빈값이 여러 개면 -1을 return 합니다.

---

### 풀이

배열의 index를 활용해서 풀었지만 다음엔 Map 을 활용해보자.

```js
function solution(array) {
  if (array.length == 1) return array[0];

  // cnt 배열 생성 & 초기화
  let cnt = Array.from({ length: Math.max(...array) + 1 }, () => 0);

  // index 값을 기준으로 갯수 저장
  array.forEach((x) => {
    cnt[x] += 1;
  });

  // 최빈 값의 index 값 찾기
  let max = Math.max(...cnt);

  let answer = cnt.map((x, i) => {
    return x == max ? i : -1;
  });

  // index 값의 갯수
  answer = answer.filter((x) => x !== -1);

  return answer.length == 1 ? answer[0] : -1;
}
```

---

## 배열의 평균값

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120817)

정수 배열 numbers가 매개변수로 주어집니다.

numbers의 원소의 평균값을 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- `Array.prototype.reduce()`

  > arr.reduce((accumulator, currentValue, currentIndex, array)[,initiialValue])

  - **accumulator** : callback 함수의 반환값을 누적합니다.
  - **currentValue** : 현재 요소
  - **currentIndex** : 현재 요소의 인덱스
  - **array** : reduce()를 호출한 배열

  initialValue를 제공하지 않으면, reduce()는 인덱스 1부터 시작해 콜백 함수를 실행하고 첫 번째 인덱스는 건너 뜁니다.

  initialValue를 제공하면 인덱스 0에서 시작합니다.

```js
function solution(numbers) {
  var sum = numbers.reduce((acc, cur) => {
    return acc + cur;
  }, 0);

  var answer = sum / numbers.length;

  return answer;
}
```
