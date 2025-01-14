---
title: 순열과 조합
author: Psmin
data: 2023-11-22 20:32:23 +0900
categories: [코딩테스트, 알고리즘]
tags: [permutation, Combination]
---

# 순열과 조합에 대해 알아보자.

---

1부터 3까지 배열이 있을 때, 이 중에서 2개를 뽑는 방식을 예시로 순열과 조합을 알아보자.

```js
const number = [1, 2, 3];
const r = 2;
```

---

## 순열 (permutation)

서로 다른 n개의 원소에서 r개를 중복없이 순서에 상관있게 선택하는 혹은 나열하는 것을 말합니다.

이 순열의 수를 기호로 `nPr`와 같이 나타냅니다.

- 결과 : `[1, 2], [1, 3], [2, 1], [2, 3], [3, 1], [3, 2]`

- 순서가 다르면 중복이 가능하므로 index는 계속 0부터 탐색합니다.
- 값의 중복은 허용하지 않기때문에 includes로 체크해주겠습니다.

```js
const tmp = [];

const backtrack = () => {
  if (tmp.length === r) {
    console.log(tmp);
    return;
  }

  for (let i = 0; i < number.length; i++) {
    if (tmp.includes(number[i])) continue;
    tmp.push(number[i]);
    backtrack();
    tmp.pop();
  }
};

backtrack();
```

---

## 조합 (Combination)

서로 다른 n개의 원소에서 r개를 중복없이 순서 상관없이 선택하는 혹은 나열하는 것을 말합니다.

이 조합의 수를 기호로 `nCr`와 같이 나타낸다.

- 결과 : `[1, 2], [1, 3], [2, 3]`

- 순서에 상관없이 중복이 불가능하므로 index를 인자로 다음 탐색에 index + 1을 보냅니다.
- index + 1을 넘기므로 현재 값 이후의 값만 선택합니다. 따라서, 값의 중복이 제거됩니다.

```js
const tmp = [];

const backtrack = (cur) => {
  if (tmp.length === r) {
    console.log(tmp);
    return;
  }

  for (let i = cur; i < number.length; i++) {
    tmp.push(number[i]);
    // i + 1을 넘기므로 중복을 제거합니다.
    backtrack(i + 1);
    tmp.pop();
  }
};

backtrack(0);
```

---

### 예제 - 프로그래머스 삼총사

[문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/131705)

1. for문

   ```js
   function solution(number) {
     var answer = 0;

     for (let i = 0; i < number.length - 2; i++) {
       for (let j = i + 1; j < number.length - 1; j++) {
         for (let z = j + 1; z < number.length; z++) {
           if (number[i] + number[j] + number[z] == 0) {
             answer++;
           }
         }
       }
     }
   }
   ```

2. 재귀 함수 활용

   ```js
   function solution(number) {
     let answer = 0;

     const combination = (current, cur) => {
       if (current.length === 3) {
         answer += current.reduce((a, b) => a + b) ? 0 : 1;
         return;
       }

       for (let i = cur; i < number.length; i++) {
         // tmp를 인자로 넘기고, push/pop을 스프레드로 한 번에 처리할 수도 있습니다.
         combination([...current, number[i]], i + 1);
       }
     };

     combination([], 0);

     return answer;
   }
   ```
