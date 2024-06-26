---
title: 프로그래머스 알고리즘 문제 - 03
author: Psmin
data: 2023-08-12 23:46:12 +0900
categories: [코딩테스트, Programmers]
tags: [programmers]
---

# Level 0

---

## 진료 순서 정하기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120835)

외과의사 머쓱이는 응급실에 온 환자의 응급도를 기준으로 진료 순서를 정하려고 합니다.  
정수 배열 emergency가 매개변수로 주어질 때 응급도가 높은 순서대로 진료 순서를 정한 배열을 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- `Array.prototype.indexOf()`

  indexOf() 메서드는 배열에서 지정된 요소를 찾을 수 있는 **첫 번째 인덱스를 반환**하고 존재하지 않으면 -1을 반환합니다.

  > arr.indexOf(searchElement[, fromIndex])

  - **searchElement** : 찾을 요소
  - **fromIndex** : 검색을 시작할 인덱스

```js
function solution(emergency) {
  let arr = Array.from(emergency).sort((a, b) => b - a);

  return emergency.map((x) => arr.indexOf(x) + 1);
}
```

---

## 제곱수 판별하기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120909)

어떤 자연수를 제곱했을 때 나오는 정수를 제곱수라고 합니다.  
정수 n이 매개변수로 주어질 때, n이 제곱수라면 1을 아니라면 2를 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- `Number.isInteger()`

  주어진 값이 정수인지 판별합니다.

```js
function solution(n) {
  return Number.isInteger(Math.sqrt(n)) ? 1 : 2;
}
```

---

## 대문자와 소문자

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120893)

문자열 my_string이 매개변수로 주어질 때, 대문자는 소문자로 소문자는 대문자로 변환한 문자열을 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- `String.prototype.toUpperCase()`

  toUpperCase() 메서드는 문자열을 대문자로 변환해 반환합니다.

- `String.prototype.toLowerCase()`

  toLowerCase() 메서드는 문자열을 소문자로 변환해 반환합니다.

```js
function solution(my_string) {
  let arr = [...my_string].map((x) =>
    x == x.toUpperCase() ? x.toLowerCase() : x.toUpperCase()
  );

  return arr.join("");
}
```

---

## 최댓값 만들기 (2)

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120862)

정수 배열 numbers가 매개변수로 주어집니다.  
numbers의 원소 중 두 개를 곱해 만들 수 있는 최댓값을 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- `Array.prototype.at()`

  정수 값을 받아, 배열에서 해당 값에 해당하는 인덱스의 요소를 반환합니다.

  양수와 음수 모두 지정할 수 있고, 음수 값의 경우 배열의 뒤에서부터 인덱스를 셉니다.

```js
function solution(numbers) {
  numbers.sort((a, b) => a - b);

  return Math.max(
    numbers.at(0) * numbers.at(1),
    numbers.at(-1) * numbers.at(-2)
  );
}
```

---

## 문자열 정렬하기 (1)

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120850)

문자열 my_string이 매개변수로 주어질 때, my_string 안에 있는 숫자만 골라 오름차순 정렬한 리스트를 return 하도록 solution 함수를 작성해보세요.

---

### 풀이

- `String.prototype.match()`

  문자열이 정규식과 매치되는 부분을 검색합니다.

  > str.match(regexp)

  - **regexp** : 정규식 개체

  match() 메서드는 정규식에 매치되는 값을 **배열로 반환**합니다.

```js
function solution(my_string) {
  return my_string
    .match(/[0-9]/g)
    .map((x) => Number(x))
    .sort((a, b) => a - b);
}
```

---

## 인덱스 바꾸기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120895)

문자열 my_string과 정수 num1, num2가 매개변수로 주어질 때, my_string에서 인덱스 num1과 인덱스 num2에 해당하는 문자를 바꾼 문자열을 return 하도록 solution 함수를 완성해보세요.

---

### 풀이

- **구조 분해 할당**

  a와 b를 교환할 때 temp에 a 값을 담고 교환하는 과정없이 한번에 교환할 수 있습니다.

```js
function solution(my_string, num1, num2) {
  let arr = [...my_string];

  [arr[num1], arr[num2]] = [arr[num2], arr[num1]];

  return arr.join("");
}
```
