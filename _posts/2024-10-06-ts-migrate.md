---
title: 점진적 마이그레이션 (ts-migrate)
author: Psmin
data: 2024-10-06 03:23:21 +0900
categories: [Project, TypeScript]
tags: [TypeScript]
---

---

## 점진적인 마이그레이션

처음부터 모든 파일을 TypeScript로 변환하지 않고, JavaScript 파일을 점진적으로 변환합니다.

```json
// tsconfig.json
{
  "compilerOptions": {
    "allowJs": true, // JS 파일을 컴파일에 포함
    "checkJs": false // JS 파일에서 타입 검사를 비활성화
  }
}
```

타입 정의 단계에서 처음에는 `any`로 타입을 설정한 후 하나씩 변환해나갑니다.

```ts
// JavaScript (변경 전)
function add(a, b) {
  return a + b;
}

// 점진적 변경
function add(a: any, b: any): any {
  return a + b;
}

// TypeScript (변경 후)
function add(a: number, b: number): number {
  return a + b;
}
```

---

## ts-migrate

JavaScript 프로젝트를 TypeScript로 마이그레이션하는 과정을 자동화하고 간소화하는 도구입니다.

- **설치**

  ```console
  npm i ts-migrate
  ```

---

### 주요 기능

- **TypeScript로 파일 변환**

  `.js` 파일을 `.ts` 또는 `.tsx`로 변환합니다.

  TypeScript가 요구하는 기본적인 문법 변환 작업을 수행합니다.

- **@ts-ignore 주석 추가**

  TypeScript 컴파일 오류를 방지하기 위해 필요한 위치에 `@ts-ignore` 또는 `any` 타입을 추가합니다.

  초기 전환 단계에서 컴파일러의 오류를 무시하고 실행 가능한 상태를 유지합니다.

- **TypeScript 구성 파일 생성**

  `tsconfig.json` 파일을 자동 생성하거나 업데이트하여 프로젝트의 TypeScript 설정을 제공합니다.

- **점진적 마이그레이션 지원**

  모든 파일을 한 번에 마이그레이션할 필요 없이, 파일 단위로 점진적으로 TypeScript로 전환할 수 있습니다.

---

### 사용법

```bash
ts-migrate migrate <directory>
```

지정한 디렉토리의 파일을 TypeScript로 변환합니다.

변환 후, 자동으로 추가된 `@ts-ignore` 주석과 타입을 점검하고 수동으로 타입 정의를 개선합니다.

---

### 예시

```ts
// Before: App.js
function App() {
  return <div>Hello, world!</div>;
}

export default App;

---

// After: App.tsx
// @ts-ignore
function App(): any {
  return <div>Hello, world!</div>;
}

export default App;

// 점진적 개선
// App.tsx
import React from 'react';

function App(): JSX.Element {
  return <div>Hello, world!</div>;
}

export default App;
```
