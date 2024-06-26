---
title: 프로그래머스 알고리즘 문제 - 04
author: Psmin
data: 2023-08-14 23:26:13 +0900
categories: [코딩테스트, Programmers]
tags: [programmers]
---

# Level 0

---

## 배열 회전시키기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120844)

정수가 담긴 배열 numbers와 문자열 direction가 매개변수로 주어집니다.  
배열 numbers의 원소를 direction방향으로 한 칸씩 회전시킨 배열을 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- `Array.prototype.pop()`

  배열에서 **마지막 요소를 제거**하고 그 요소를 반환합니다.

- `Array.prototype.push()`

  새로운 요소를 **배열의 끝에 추가**하고, 배열의 새로운 길이를 반환합니다.

- `Array.prototypr.shift()`

  배열에서 **첫 번째 요소를 제거**하고, 제거된 요소를 반환합니다.

- `Array.prototype.unshift()`

  새로운 요소를 **배열의 맨 앞에 추가**하고, 새로운 길이를 반환합니다.

```js
function solution(numbers, direction) {
  direction === "right"
    ? numbers.unshift(numbers.pop())
    : numbers.push(numbers.shift());
  return numbers;
}
```

---

## 2차원으로 만들기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120842)

정수 배열 num_list와 정수 n이 매개변수로 주어집니다. num_list를 다음 설명과 같이 2차원 배열로 바꿔 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- `Array.prototype.slice()`

  어떤 배열의 begin 부터 end 까지(end 미포함)에 대한 **얕은 복사본을 새로운 배열 객체**로 반환합니다.

  - **_원본 배열은 바뀌지 않습니다._**

  > arr.slice([begin[, end]])

  - **begin** : 0을 시작으로 하는 추출 시작점에 대한 인덱스

  - **end** : 추출을 종료 할 0 기준 인덱스, slice 는 end 인덱스를 제외하고 추출합니다.

- `Array.prototype.splice()`

  배열의 기존 요소를 삭제 또는 교체하거나 새 요소를 추가하여 **배열의 내용을 변경**합니다.

  - **_반환값은 제거된 요소들의 배열 !_**

  > array.splice(start[, deleteCount[, item1[, item2[, ...]]]])

  - **start** : 배열의 변경을 시작할 인덱스
  - **deleteCount** : 배열에서 제거할 요소의 갯수
  - **item** : 배열에 추가할 요소

```js
function solution(num_list, n) {
  var answer = [];

  while (num_list.length) {
    answer.push(num_list.splice(0, n));
  }

  return answer;
}
```

---

## 합성수 찾기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120846)

약수의 개수가 세 개 이상인 수를 합성수라고 합니다.  
자연수 n이 매개변수로 주어질 때 n이하의 합성수의 개수를 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- `Set.prototype.add()`

  Set 개체의 맨 뒤에 주어진 value의 **새 요소를 추가**합니다.

- `Set.prototype.size`

  size 접근자 속성은 **Set 객체의 원소 수를 반환**합니다.

```js
function solution(n) {
  var answer = new Set();

  for (var i = 2; i <= n; i++) {
    for (var j = 2; j <= Math.sqrt(i); j++) {
      if (i % j === 0) answer.add(i);
    }
  }

  return answer.size;
}
```

---

## 중복된 문자 제거

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120888)

문자열 my_string이 매개변수로 주어집니다.  
my_string에서 중복된 문자를 제거하고 하나의 문자만 남긴 문자열을 return하도록 solution 함수를 완성해주세요.

---

### 풀이

문자열을 Set에 담고 배열화해준후 Join하면 중복이 제거된 문자열을 얻을 수 있습니다.

```js
function solution(my_string) {
  return [...new Set(my_string)].join("");
}
```

---

## 이진수 더하기

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/120885)

이진수를 의미하는 두 개의 문자열 bin1과 bin2가 매개변수로 주어질 때, 두 이진수의 합을 return하도록 solution 함수를 완성해주세요.

---

### 풀이

- `parseInt()`

  문자열 인자를 파싱하여 **특정 진수(수의 진법 체계에서 기준이 되는 값)의 정수를 반환**합니다.

  > parseInt(string, radix)

  - **string** : 파싱할 값, 문자열이 아닐 경우 ToString 추상 연산을 사용해 문자열로 변환합니다. 문자열의 선행 공백은 무시합니다.
  - **radix** : string의 진수를 나타내는 2부터 36까지의 정수, Number 자료형이 아닌 경우 Number로 변환합니다.

- `toString()`

  문자열을 반환하는 object의 대표적인 방법입니다.

- `toString([radix])`

  숫자 및 BigInts의 경우 toString()은 선택적으로 기수(radix)를 매개변수로 취합니다. 기수의 값은 최소 2부터 36까지입니다.

  ```js
  let baseTenInt = 10;
  console.log(baseTenInt.toString(2));
  // "1010"이 출력됩니다

  let bigNum = BigInt(20);
  console.log(bigNum.toString(2));
  // "10100"이 출력됩니다
  ```

```js
function solution(bin1, bin2) {
  return (parseInt(bin1, 2) + parseInt(bin2, 2)).toString(2);
}
```
