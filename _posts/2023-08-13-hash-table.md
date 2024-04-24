---
title: 해시 테이블 Hash Table
author: Psmin
data: 2023-08-13 13:26:53 +0900
categories: [코딩테스트, 자료구조]
tags: [Hash Table]
---

# 해시 테이블에 대해 알아보자.

---

## 해시 테이블 Hash Table

![hash-table](/assets/img/hash-table.png){: .normal }

해시테이블은 **Key 값**을 입력 받아 **해시함수를 통해 변경한 Hash 값**을 **Value 값과 쌍으로 저장**한 형태의 자료구조입니다.

---

## 해시 함수 Hash Function

![hash-function](/assets/img/hash-function.png){: .normal }

입력받은 key를 고정된 길이의 해시로 변경합니다.

다양한 길이를 가진 키를 고정된 길이의 해시로 변경해 저장소를 효율적으로 운영할 수 있도록 합니다.

---

## 해싱 Hashing

임의의 길이의 값을 **해시 함수(Hash Function)**을 사용하여 고정된 크기의 값으로 변환하는 작업을 말합니다.

---

## 해시 테이블의 시간 복잡도

해시 테이블은 데이터를 검색할 때 사용할 **key 값과 Value값 이 한 쌍으로 존재**하며, **key 값이 해시 함수를 통해 인덱스로 변환**하기 때문에 `시간 복잡도가 O(1)`로 빠릅니다.

즉, 일반 배열에서 데이터를 검색할 때는 시간 복잡도가 **O(N)**으로 오래걸리자만 해시 테이블의 경우 **O(1) 상수 시간**이기 때문에 빠르게 처리할 수 있습니다.

따라서, 매우 빠른 데이터 검색을 위한 컴퓨터 소프트웨어에서 사용됩니다.

---

## 해시 맵 Hash Map

자바스크립트에서 해시 테이블은 `Object`, `Map`, `Set`을 사용합니다.

이번 포스팅에서는 `Map`을 활용한 Hash Map을 구현해보겠습니다.

---

## 해시 맵의 추상 자료형 ADT

- **set** : 해시 테이블에 데이터 추가
- **get** : 특정 데이터 출력
- **remove** : 특정 데이터 제거
- **clear** : 해시 테이블 안의 모든 데이터를 삭제
- **size** : Map의 크기

---

## 구현 코드

Map 자체로도 사용가능 하지만 `Class`로 구현해보겠습니다.

```js
class HashMap {
  constructor() {
    this._map = new Map();
  }

  set(key, value) {
    this._map.set(key, value);
  }

  get(key) {
    return this._map.get(key);
  }

  remove(key) {
    return this._map.delete(key);
  }

  clear() {
    return this._map.clear();
  }

  size() {
    return this._map.size();
  }
}
```

---

## Hash Map 장단점

1. Map 객체는 `iterable` 이므로 `for .. of 문`을 통해 반복문 가능

2. 삽입, 삭제에서 Object보다 나은 성능

3. Key 값에 여러 자료형 가능

4. JSON 으로의 변환 지원 X
