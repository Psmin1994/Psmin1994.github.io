---
title: Error Handler (Middleware)
author: Psmin
data: 2023-06-01 13:23:08 +0900
categories: [Knowledge, NodeJS]
tags: [Error Handler]
---

# 에러 핸들러를 미들웨어로 구현해보자.

프로젝트의 전역 에러 핸들러를 미들웨어로 정의하여, 모든 라우트, 컨트롤러에서 발생하는 오류를 일괄적으로 처리해보자.

---

## 장점

- **중복 코드 방지**

  각 컨트롤러나 서비스에서 일일이 try-catch 블록을 사용하는 대신, 미들웨어에서 한 번에 처리할 수 있습니다.

- **일관된 응답 구조**

  예외가 발생했을 때 응답 형태(status, message, error 등)를 통일할 수 있습니다.

- **로깅 및 디버깅 용이**

  에러 발생 시 로깅 시스템과 연계하여 한 곳에서 오류를 기록하고 관리할 수 있습니다.

- **NestJS 전환**

  NestJS에서도 **ExceptionFilter**를 활용하여 유사한 방식으로 에러를 핸들링하므로, 미리 구조를 잡아두면 전환이 용이합니다.

---

## errorHandler.ts

```ts
import { Request, Response, NextFunction } from "express";

// 커스텀 에러 타입 정의 (옵션)
class AppError extends Error {
  statusCode: number;
  constructor(message: string, statusCode: number) {
    super(message);
    this.statusCode = statusCode;
    Error.captureStackTrace(this, this.constructor);
  }
}

// 에러 핸들링 미들웨어
const errorHandler = (
  err: any,
  req: Request,
  res: Response,
  next: NextFunction
) => {
  const statusCode = err.statusCode || 500;
  const message = err.message || "서버 오류가 발생했습니다.";

  console.error(`[ERROR] ${req.method} ${req.url} - ${message}`);

  res.status(statusCode).json({
    success: false,
    message,
    ...(process.env.NODE_ENV === "development" ? { stack: err.stack } : {}),
  });
};

export { errorHandler, AppError };
```
