---
title: Typescript
author: Psmin
data: 2024-10-01 15:23:17 +0900
categories: [Knowledge, Typescript]
tags: [Typescript]
---

## Typescript

Typescript는 Javascript의 상위 집합으로 **JavaScript의 모든 기능을 포함하면서 추가적인 기능을 제공**합니다.

Javascript 엔진을 사용하며 **변수의 타입을 정의하고 프로그래밍**을 하면 **Javascript로 컴파일**되어 실행할 수 있습니다.

이 때, 컴파일은 브라우저에서 실행하기위해 **Typescript 컴파일러(TSC)를 사용하여 Typescript로 작성된 코드를 Javascript 코드로 변환해주는 과정**을 말합니다.

---

## 주요 특징

- **정적 타입 시스템**

  변수, 함수, 객체 등에 타입을 명시적으로 정의할 수 있습니다.

  컴파일 단계에서 타입 오류를 확인하여 런타임 오류를 줄일 수
  있습니다.

  ```js
  let name: string = "Alice";
  let age: number = 25;
  ```

- **최신 JavaScript 기능 지원**

  ES6+ 이상의 JavaScript 문법을 사용할 수 있습니다.

  TypeScript는 JavaScript로 트랜스파일(transpile)되므로 브라우저 호환성이 보장됩니다.

- **객체 지향 프로그래밍(OOP) 지원**

  클래스, 인터페이스, 상속 등을 활용한 객체 지향 설계를 쉽게 구현할 수 있습니다.

  ```js
  class Person {
    constructor(private name: string, private age: number) {}

    greet() {
      console.log(`Hello, my name is ${this.name}.`);
    }
  }
  ```

- **JavaScript 호환성**

  기존 JavaScript 코드를 그대로 TypeScript에서 사용할 수 있습니다.

  점진적으로 타입을 추가하여 마이그레이션이 가능합니다.

- **도구 지원 강화**

  타입 정의 덕분에 **코드 자동 완성**, **리팩토링**, **디버깅**이 편리해집니다.

  많은 IDE와 통합되어 생산성을 높여줍니다.

- **타입 정의 파일(.d.ts)**

  외부 라이브러리(JavaScript 기반)에서도 타입을 사용할 수 있도록 타입 정의 파일을 제공합니다.

  **@types 패키지**를 통해 인기 라이브러리들의 타입을 설치할 수 있습니다.

---

## 장점

- **안정성** : 정적 타입 기반으로 컴파일 과정에서 에러 발견
- **생산성** : IDE의 자동완성 및 타입 체킹 기능
- **가독성** : 타입 선언으로 문서화 역할

---

## 단점

- **초기 학습 난이도** : 타입 시스템 학습
- **빌드 시간** : 컴파일 과정 필요
