---
title: 에러 처리
author: Psmin
data: 2025-02-03 16:36:45 +0900
categories: [Knowledge, NestJS]
tags: [Error Handler]
---

# App Router 기반 Next.js에서 에러 처리하는 방법에 대해 알아보자.

---

## 에러 처리

`error.tsx` 파일을 통해 라우트 단위로 에러 처리를 할 수 있습니다.

App Router는 클라이언트 컴포넌트에서만 Error Boundary를 지원하기 때문에 클라이언트 컴포넌트만 가능 ('use client')

**디렉토리 구조**

```ts
app/
  page.tsx                 ← 홈
  error.tsx                ← 전역 에러 처리
  dashboard/
    page.tsx               ← 이 안에서 에러 발생
    error.tsx              ← 경로 전용 에러 처리
```

---

## 우선 순위

- 가까운 `error.tsx`가 우선적으로 에러를 처리

- `error.tsx`도 아예 없는 경우

  Next.js는 내부적으로 기본 오류 페이지를 보여줍니다.

  > 개발 모드에선 stack trace, 프로덕션에선 흰 화면

---

## not-found.tsx

App Router에서는 404 페이지를 처리하기 위해 `not-found.tsx` 파일을 사용합니다.

```tsx
// app/not-found.tsx
export default function NotFoundPage() {
  return (
    <div className="text-center mt-10 text-xl text-red-600">
      요청하신 페이지를 찾을 수 없습니다. 😞
      <a href="/" className="text-blue-600">
        홈으로 돌아가기
      </a>
    </div>
  );
}
```

not-found 역시 가까운 `not-found.tsx`가 먼저 처리합니다.

`notFound()`는 개별 경로 내에서 404 처리할 때 주로 사용하며, 특정 조건에서만 404를 처리하고 싶을 때도 사용합니다.

```tsx
// app/blog/[slug]/page.tsx
import { notFound } from "next/navigation";

export default async function BlogPage({ params }) {
  const res = await fetch(`https://api.example.com/posts/${params.slug}`);

  if (!res.ok) {
    // 데이터가 없을 경우 404 페이지로 이동
    return notFound(); // 자동으로 404 처리
  }

  const post = await res.json();
  return <div>{post.title}</div>;
}
```

---

## 서버에서 에러 처리하는 경우

서버에서 다양한 에러들을 처리한 후, 응답을 클라이언트로 반환하는 방식이라면 **응답 상태에 따른 분기 처리**가 필요합니다.

```tsx
// app/user/[id]/page.tsx
export default async function UserPage({ params }) {
  const res = await fetch(`/api/users/${params.id}`);

  if (!res.ok) {
    // 상태코드 : 404
    if (res.status === 404) {
      return <div>사용자를 찾을 수 없습니다.</div>;
    }

    // 상태코드 : 500, 400 등
    return <div>서버에서 오류가 발생했습니다. 다시 시도해주세요.</div>;
  }

  const data = await res.json();
  return <div>{data.name}</div>;
}
```

---

### 상태 코드별로 컴포넌트로 분리

`/components/ErrorMessage.tsx`을 커스텀해 일관되게 표시할 수 있습니다.

```tsx
// components/ErrorMessage.tsx

type ErrorMessageProps = {
  message: string;
};

export default function ErrorMessage({ message }: ErrorMessageProps) {
  return <div className="text-red-600 text-center mt-10">❗ {message}</div>;
}
```
