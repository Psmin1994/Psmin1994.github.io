---
title: generate
author: Psmin
data: 2025-01-12 18:13:17 +0900
categories: [Knowledge, NestJS]
tags: [Nest, generate]
---

## generate 명령어

`nest generate` OR `nest g` 명령어를 사용해 여러가지 리소스나 모듈을 자동 생성할 수 있습니다.

---

## 주요 명령어

- **module**

  ```bash
  nest generate module [module-name]
  # 또는 축약형
  nest g mo [module-name]
  ```

- **controller**

  ```bash
  nest generate controller  [controller -name]
  # 또는 축약형
  nest g co [controller -name]
  ```

- **service**

  ```bash
  nest generate service [service-name]
  # 또는 축약형
  nest g s [service-name]
  ```

- **guard**

  ```bash
  nest generate guard [guard-name]
  # 또는 축약형
  nest g gu [guard-name]
  ```

- **pipe**

  ```bash
  nest generate pipe [pipe-name]
  # 또는 축약형
  nest g pi [pipe-name]
  ```

- **interceptor**

  ```bash
  nest generate interceptor [interceptor-name]
  # 또는 축약형
  nest g in [interceptor-name]
  ```

- **decorator**

  ```bash
  nest generate decorator [decorator-name]
  # 또는 축약형
  nest g de [decorator-name]
  ```

- **resource**

  모듈, 컨트롤러, 서비스, 그리고 관련된 DTO, 파이프, 가드 등을 자동으로 생성합니다

  ```bash
  nest generate resource [resource-name]
  # 또는 축약형
  nest g res [resource-name]
  ```
