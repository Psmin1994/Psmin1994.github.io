---
title: 백준 연결리스트 문제 - 01
author: Psmin
data: 2023-08-17 17:27:33 +0900
categories: [코딩테스트, Backjoon]
tags: [Backjoon]
---

## 10845번 문제 - 큐

[백준 10845번 문제](https://www.acmicpc.net/problem/10845)

---

### 풀이

`console.log`의 호출이 늘어날 수록 시간이 급격하게 증가 => 시간초과

- 출력은 변수에 담아서 `join()`해서 출력하자.

- 배열을 이용한 큐 구현

```js
const filePath = process.platform !== "linux" ? "./test.txt" : "dev/stdin";

let input = require("fs").readFileSync(filePath).toString().trim().split("\n");

let n = +input[0];
let queue = [];
let answer = [];

for (let i = 1; i <= n; i++) {
  let arr = input[i].trim().split(" ");

  switch (arr[0]) {
    case "push":
      queue.push(arr[1]);
      break;

    case "pop":
      answer.push(queue.length ? queue.shift() : -1);
      break;

    case "size":
      answer.push(queue.length);
      break;

    case "empty":
      answer.push(queue.length ? 0 : 1);
      break;

    case "front":
      answer.push(queue.length ? queue[0] : -1);
      break;

    case "back":
      answer.push(queue.length ? queue[queue.length - 1] : -1);
      break;
  }
}

console.log(answer.join("\n"));
```
