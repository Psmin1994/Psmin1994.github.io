---
title: 부동소수점 Floating Point
author: Psmin
data: 2023-06-14 20:02:12 +0900
categories: [Knowledge, Javascript]
tags: [부동소수점]
---

# 자바스크립트에서 소수점 연산을 해보자.

---

## 부동소수점

Javascript는 숫자에 대해 **64비트 부동소수점** 표현을 사용합니다.

![js-number](/assets/img/js-number.png){: .normal}

- **부호 Sign**

  양수 음수를 구분합니다.

- **지수 Exponent**

  총 11 비트로 지수값을 나타냅니다.

- **가수 Mantissa**

  총 52비트로 가수부분을 나타냅니다.

---

## 소수점 계산 오류

컴퓨터는 2진법을 사용해 부동소수점 방식으로 숫자를 저장합니다.

![floating-ex](/assets/img/floating-ex.png){: .normal}

이러한 문제가 발생하는 이유는 10진법을 2진법으로 변환하는 과정에서 오차가 발생하기 때문입니다.

---

### 해결방법

- **toFixed() 메소드**

  Number 객체의 toFixed() 메소드를 활용합니다.

  인수만큼 자리수를 반올림하여 문자열로 반환해주는 함수입니다.

  ![floating-tofixed](/assets/img/floating-tofixed.png){: .normal}

  > 문자열로 반환되는 것을 생각하자.

- **Math.round()**

  Math 객체의 round() 메소드를 활용합니다.

  반올림 메소드로 인수를 반올림한 후 가까운 정수로 반환해주는 함수입니다.

  ![floating-round](/assets/img/floating-round.png){: .normal}

- **Number.EPSILON**

  Number객체의 EPSILON은 두 개의 표현 가능한 숫자 사이의 가장 작은 간격을 반환합니다.

  ![floating-epsilon](/assets/img/floating-epsilon.png){: .normal}

  ```js
  const numberEqual(x, y) {
   return Math.abs(x - y) < Number.EPSILON
  }

  numberEqual(0.1 + 0.2, 0.3); // true
  ```

  두 수의 차이가 Number.EPSILON보다 작은 지 검사해 같은 지 확인하는 방법도 있습니다.
