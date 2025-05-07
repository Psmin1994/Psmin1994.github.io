---
title: guard, pipe
author: Psmin
data: 2025-01-21 22:07:02 +0900
categories: [Knowledge, NestJS]
tags: [Guard, Pipe]
---

# Nest에서 Guard 와 Pipe의 역할을 알아보자.

---

## 가드 Guard

요청이 컨트롤러의 핸들러에 도달하기 전에 **특정 조건을 만족하는지 검사**하고, 해당 요청을 처리할지 말지를 결정하는 미들웨어와 비슷한 역할을 합니다.

주로 **인증 및 권한 부여 (Authorization)** 에 사용됩니다.

> NestJS의 가드는 Express의 미들웨어와 유사하지만, 더 강력한 제어가 가능합니다.

true를 반환하면 요청을 계속 진행하고, false를 반환하면 요청을 차단합니다.

---

### 간단 예시

- **간단한 인증 가드 구현**

  ```ts
  import { Injectable, CanActivate, ExecutionContext } from "@nestjs/common";

  @Injectable()
  export class AuthGuard implements CanActivate {
    canActivate(context: ExecutionContext): boolean {
      const request = context.switchToHttp().getRequest();

      return request.headers.authorization ? true : false; // 단순 인증 여부 체크
    }
  }
  ```

- **컨트롤러에 적용**

  컨트롤러에서 특정 핸들러(메서드)에 `@UseGuards`를 적용할 수 있습니다.

  ```ts
  import { Controller, Get, UseGuards } from "@nestjs/common";
  import { AuthGuard } from "./auth.guard";

  @Controller("users")
  export class UserController {
    @Get()
    @UseGuards(AuthGuard)
    // 이 핸들러는 AuthGuard를 통과해야 실행됨
    findAll() {
      return "User list";
    }
  }
  ```

---

### 전역 가드 적용

`main.ts`에서 `app.useGlobalGuards()`를 사용하면 전체 애플리케이션에 적용할 수 있습니다.

```ts
import { NestFactory } from "@nestjs/core";
import { AppModule } from "./app.module";
import { AuthGuard } from "./auth.guard";

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  app.useGlobalGuards(new AuthGuard());

  await app.listen(3000);
}
bootstrap();
```

---

## Pipe

요청 데이터의 **유효성 검사 (Validation)** 및 **변환 (Transformation)** 을 수행하는 역할을 합니다.

---

### 간단 예시

- **간단한 파이프 구현**

  ```ts
  import {
    PipeTransform,
    Injectable,
    ArgumentMetadata,
    BadRequestException,
  } from "@nestjs/common";

  @Injectable()
  export class ParseIntPipe implements PipeTransform {
    transform(value: any, metadata: ArgumentMetadata) {
      const val = parseInt(value, 10);

      if (isNaN(val)) {
        throw new BadRequestException("Validation failed");
      }

      return val;
    }
  }
  ```

- **컨트롤러에 적용**

  ```ts
  import { Controller, Get, Param, UsePipes } from "@nestjs/common";
  import { ParseIntPipe } from "./parse-int.pipe";

  @Controller("users")
  export class UserController {
    @Get(":id")
    @UsePipes(ParseIntPipe)
    // id 값을 숫자로 변환
    findOne(@Param("id") id: number) {
      return `User with ID ${id}`;
    }
  }
  ```

---

### 전역 파이프 적용

`main.ts`에서 `app.useGlobalPipes()`를 사용하여 전역으로 파이프를 적용할 수 있습니다.

```ts
import { NestFactory } from "@nestjs/core";
import { AppModule } from "./app.module";
import { ValidationPipe } from "@nestjs/common";

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  app.useGlobalPipes(new ValidationPipe());

  await app.listen(3000);
}
bootstrap();
```
