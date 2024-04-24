---
title: 연결리스트 Linked List
author: Psmin
data: 2023-08-09 13:26:53 +0900
categories: [코딩테스트, 자료구조]
tags: [Linked List]
---

# 연결리스트에 대해 알아보자.

---

## 연결 리스트 Linked List

연결 리스트란 각 노드가 **데이터와 포인터**를 갖으며 **한 줄로 연결되어 있는 것**처럼 데이터를 관리하는 방식의 자료 구조입니다.

![linked-list](/assets/img/linked-list.png){: .normal }

---

## 연결 리스트의 종류

- **단순 연결 리스트 Singly Linked List**

  기본적인 연결 리스트로 **첫 번째 노드 (Head)부터 마지막 노드 (Tail)까지 단방향**으로 이어집니다.

  각 노드에 자료 공간과 한 개의 포인터 공간이 있고, 각 노드의 포인터는 다음 노드를 가리킵니다.

  ![singly-linked-list](/assets/img/singly-linked-list.png){: .normal }

- **이중 연결 리스트 Doubly Linked List**

  단순 연결 리스트에서 **이전 노드를 가리키는 포인터가 추가**되어 이전 노드로는 갈 수 없었던 단점을 보완한 형태입니다.

  ![doubly-linked-list](/assets/img/doubly-linked-list.png){: .normal }

- **원형 연결 리스트 Circular Linked List**

  단순 열결 리스트에서 **마지막 노드 (Tail)의 포인터가 첫 번째 노드 (Head)를 가리키는 형태**입니다.

  ![circular-linked-list](/assets/img/circular-linked-list.png){: .normal }

---

## 단순 연결 리스트의 추상 자료형 ADT

- **addFirst** : 첫 번째 노드 앞에 노드를 추가
- **addLast** : 마지막 노드 뒤에 노드를 추가
- **add** : 특정 위치 노드 뒤에 노드 추가
- **removeFirst** : 첫 번째 노드 삭제
- **removeLast** : 마지막 노드 삭제
- **remove** : 특정 위치 노드 삭제
- **get** : 특정 위치 노드 조회
- **set** : 특정 위치 노드 변경
- **printNodeAll** : 연결 리스트 안의 모드 노드의 데이터 출력
- **size** : 연결 리스트의 노드 갯수
- **isEmpty** : 연결 리스트가 비어있는 지 확인

---

### 구현 코드

```js
class Node {
  constructor(data) {
    this.data = data || null;
    this.next = null;
  }
}

class SinglyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.size = 0;
  }

  addFirst(newData) {
    let newNode = new Node(newData);

    if (this.head) {
      newNode.next = this.head;
      this.head = newNode;
    } else {
      this.head = newNode;
      this.tail = newNode;
    }

    this.size++;

    return newNode;
  }

  addLast(newData) {
    let newNode = new Node(newData);

    if (this.tail) {
      this.tail.next = newNode;
      this.tail = newNode;
    } else {
      this.head = newNode;
      this.tail = newNode;
    }

    this.size++;

    return newNode;
  }

  add(newData, index) {
    if (index == undefined || index < 0 || index > this.size) return -1;

    let newNode = new Node(newData);

    if (index == 0) {
      newNode.next = this.head;
      this.head = newNode;
    } else {
      let curNode = this.head;

      for (let i = 0; i < index - 1; i++) {
        curNode = curNode.next;
      }

      newNode.next = curNode.next;
      curNode.next = newNode;
    }

    this.size++;

    return newNode;
  }

  removeFirst() {
    if (this.head) {
      let rmNode = this.head;

      this.head = rmNode.next;

      this.size--;

      return rmNode;
    } else {
      return -1;
    }
  }

  removeLast() {
    let curNode = this.head;

    for (let i = 0; i < this.size - 2; i++) {
      curNode = curNode.next;
    }

    let rmNode = curNode.next;

    this.tail = curNode;
    curNode.next = null;
    this.size--;

    return rmNode;
  }

  remove(index) {
    if (index == 0) {
      this.removeFirst();
    } else {
      let curNode = this.head;

      for (let i = 0; i < index - 1; i++) {
        curNode = curNode.next;
      }

      let rmNode = curNode.next;

      curNode.next = rmNode.next;
      this.size--;

      return rmNode;
    }
  }

  get(index) {
    if (index == 0) {
      return this.head;
    } else {
      let curNode = this.head;

      for (let i = 0; i < index; i++) {
        curNode = curNode.next;
      }

      return curNode;
    }
  }

  set(newData, index) {
    if (index == 0) {
      this.head.data = newData;

      return this.head;
    } else {
      let curNode = this.head;

      for (let i = 0; i < index; i++) {
        curNode = curNode.next;
      }

      curNode.data = newData;

      return curNode;
    }
  }

  printNodeAll() {
    let curNode = this.head;
    let result = "";

    for (let i = 0; i < this.size - 1; i++) {
      result += `[ ${curNode.data} ] -> `;
      curNode = curNode.next;
    }

    result += `[ ${curNode.data} ]`;

    console.log(result);
  }

  size() {
    return this.size;
  }

  isEmpty() {
    return this.size == 0 ? true : false;
  }
}
```

---

## 이중 연결 리스트 구현 코드

```js
class Node {
  constructor(data) {
    this.data = data || null;
    this.next = null;
    this.prev = null;
  }
}

class DoublyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.size = 0;
  }

  addFirst(newData) {
    let newNode = new Node(newData);

    if (this.head) {
      this.head.prev = newNode;
      newNode.next = this.head;

      this.head = newNode;
    } else {
      this.head = newNode;
      this.tail = newNode;
    }

    this.size++;

    return newNode;
  }

  addLast(newData) {
    let newNode = new Node(newData);

    if (this.tail) {
      this.tail.next = newNode;
      newNode.prev = this.tail;

      this.tail = newNode;
    } else {
      this.head = newNode;
      this.tail = newNode;
    }

    this.size++;

    return newNode;
  }

  add(newData, index) {
    if (index == undefined || index < 0 || index > this.size) return -1;

    let newNode = new Node(newData);

    if (index == 0) {
      this.head.prev = newNode;
      newNode.next = this.head;

      this.head = newNode;
    } else {
      let curNode = this.head;

      for (let i = 0; i < index - 1; i++) {
        curNode = curNode.next;
      }

      newNode.next = curNode.next;
      newNode.next.prev = newNode;

      curNode.next = newNode;
      newNode.prev = curNode;
    }

    this.size++;

    return newNode;
  }

  removeFirst() {
    if (this.head) {
      let rmNode = this.head;

      this.head = rmNode.next;
      this.head.prev = null;

      this.size--;

      return rmNode;
    } else {
      return -1;
    }
  }

  removeLast() {
    let rmNode = this.tail;

    this.tail = rmNode.prev;
    this.tail.next = null;

    this.size--;

    return rmNode;
  }

  remove(index) {
    if (index == 0) {
      this.removeFirst();
    } else {
      let curNode = this.head;

      for (let i = 0; i < index - 1; i++) {
        curNode = curNode.next;
      }

      let rmNode = curNode.next;

      curNode.next = rmNode.next;
      curNode.next.prev = curNode;

      this.size--;

      return rmNode;
    }
  }

  get(index) {
    if (index == 0) {
      return this.head;
    } else {
      let curNode = this.head;

      for (let i = 0; i < index; i++) {
        curNode = curNode.next;
      }

      return curNode;
    }
  }

  set(newData, index) {
    if (index == 0) {
      this.head.data = newData;

      return this.head;
    } else {
      let curNode = this.head;

      for (let i = 0; i < index; i++) {
        curNode = curNode.next;
      }

      curNode.data = newData;

      return curNode;
    }
  }

  printNodeAll() {
    let curNode = this.head;
    let result = "";

    for (let i = 0; i < this.size - 1; i++) {
      result += `[ ${curNode.data} ] -> `;
      curNode = curNode.next;
    }

    result += `[ ${curNode.data} ]`;

    console.log(result);
  }

  size() {
    return this.size;
  }

  isEmpty() {
    return this.size == 0 ? true : false;
  }
}
```
