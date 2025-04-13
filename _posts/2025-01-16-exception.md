---
title: 에러 처리 방법
author: Psmin
data: 2025-01-16 19:36:18 +0900
categories: [Knowledge, NestJS]
tags: [Exception]
---

# NestJS에서의 에러 처리 방식에 대해 알아보자.

---

## HttpException

NestJS에서 제공하는 기본적인 예외 클래스입니다.

**HTTP 상태 코드와 함께 사용자 지정 응답을 반환**할 수 있습니다.

- **기본 구조**

  ```ts
  new HttpException(response: string | object, status: HttpStatus);

  ```

  - **response** : 응답으로 반환할 메시지(문자열 또는 객체)

  - **status** : HTTP 상태 코드 (HttpStatus enum 사용 가능)

---

## HttpStatus

NestJS에서 제공하는 **HTTP 상태 코드 열거형 (enum)**입니다.

HTTP 표준 상태 코드를 쉽게 사용할 수 있도록 정의되어 있습니다.

- `HttpStatus.OK` : 200, 성공
- `HttpStatus.CREATED` : 201, 생성됨
- `HttpStatus.BAD_REQUEST` : 400, 잘못된 요청
- `HttpStatus.UNAUTHORIZED` : 401, 인증 필요
- `HttpStatus.FORBIDDEN` : 403, 접근 금지
- `HttpStatus.NOT_FOUND` : 404, 찾을 수 없음
- `HttpStatus.INTERNAL_SERVER_ERROR` : 500, 서버 내부 오류

---

## 기본적인 에러 처리

컨트롤러에서 `throw new HttpException()`을 호출하면 자동으로 적절한 응답을 반환합니다.

```ts
import { Controller, Get, HttpException, HttpStatus } from "@nestjs/common";

@Controller("example")
export class ExampleController {
  @Get()
  getExample() {
    throw new HttpException("Forbidden", HttpStatus.FORBIDDEN);
  }
}
```

`403 Forbidden` 상태 코드와 함께 **"Forbidden"** 메시지가 반환됩니다.

```
{
  "statusCode": 403,
  "message": "Forbidden"
}
```

---

## 커스텀 예외 클래스

`HttpException`을 확장해서 커스텀 예외를 만들 수 있습니다.

```ts
// 커스텀 에러 클래스 생성
import { HttpException, HttpStatus } from "@nestjs/common";

export class CustomException extends HttpException {
  constructor(message: string, statusCode: HttpStatus) {
    super(message, statusCode);

    Error.captureStackTrace(this, this.constructor);
  }
}

// 컨트롤러에서 사용
import { Controller, Get } from "@nestjs/common";
import { CustomException } from "./custom.exception";

@Controller("example")
export class ExampleController {
  @Get()
  getExample() {
    throw new CustomException("User not found", 404);
  }
}
```

---

## ExceptionFilter

NestJS에서는 `ExceptionFilter`를 사용하면 에러를 **일괄적으로 처리**할 수 있습니다.

또한, 애플리케이션에서 발생하는 예외를 **커스터마이징하여 처리**할 수 있습니다.

---

### 기본 사용방법

`ExceptionFilter`는 **ExceptionFilter 인터페이스를 구현한 클래스**로 작성합니다.

이 클래스는 `catch` 메서드를 구현해야 하며, 해당 메서드는 발생한 예외와 그 예외에 대한 응답을 처리합니다.

```ts
import { ExceptionFilter, Catch, ArgumentsHost } from "@nestjs/common";
import { Response } from "express";

@Catch() // 모든 예외를 처리하는 필터
export class AllExceptionFilter implements ExceptionFilter {
  catch(err: any, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const status = err.status || 500;

    response.status(status).json({
      message: err.message || "Internal server error",
      timestamp: new Date().toISOString(),
    });
  }
}
```

---

작성된 필터는 컨트롤러에서 `UseFilters`를 사용해 **특정 핸들러에 적용**할 수 있습니다.

```ts
import {
  Controller,
  Get,
  UseFilters,
  HttpException,
  HttpStatus,
} from "@nestjs/common";
import { AllExceptionFilter } from "./filters/exception.filter";

@Controller("example")
export class ExampleController {
  @Get()
  @UseFilters(AllExceptionFilter) // 특정 핸들러에만 적용
  getExample() {
    throw new HttpException("권한이 없습니다.", HttpStatus.FORBIDDEN);
  }
}
```

---

모든 컨트롤러에 적용하려면 `main.ts` 파일에 등록합니다.

```ts
import { NestFactory } from "@nestjs/core";
import { AppModule } from "./app.module";
import { AllExceptionFilter } from "./filters/http-exception.filter";

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalFilters(new AllExceptionFilter()); // 글로벌 필터 등록
  await app.listen(3000);
}
bootstrap();
```

---

### 커스텀 예외 필터

애플리케이션에서 사용자 정의 예외를 생성하고 이를 처리하는 필터를 만들 수도 있습니다.

`@Catch()` 데코레이터에 **예외 클래스를 지정**하여 특정 예외만 처리할 수 있습니다.

```ts
// src/common/exception/app-error.ts
export class AppError extends Error {
  constructor(public message: string, public statusCode: number) {
    super(message);
    this.statusCode = statusCode;
    this.name = this.constructor.name;
  }
}

// src/common/exception/global-exception.filter.ts
import { Catch, ExceptionFilter, ArgumentsHost } from "@nestjs/common";
import { Response } from "express";
import { AppError } from "./app-error";

@Catch(AppError)
export class CustomExceptionFilter implements ExceptionFilter {
  catch(err: AppError, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const res = ctx.getResponse<Response>();

    // 개발 환경에서는 콘솔에 출력
    if (process.env.NODE_ENV === "dev") {
      console.error(err.stack);
    }

    res.status(err.statusCode).json({
      message: err.message,
      errorType: "CustomException",
      timestamp: new Date().toISOString(),
    });
  }
}
```
