---
title: App Router
author: Psmin
data: 2025-02-05 22:07:02 +0900
categories: [Knowledge, NextJS]
tags: [App Router]
---

## App Router

Next.js 13 이상에서 도입된 새로운 라우팅 방식으로, 기존의 pages 디렉토리 기반 라우팅(Pages Router) 대신 app 디렉토리를 사용합니다.

파일 시스템 기반 라우팅은 유지하면서, 더 나은 **레이아웃 구성**, **서버 컴포넌트**, **로딩 상태** 등 다양한 기능을 제공합니다.

---

### React Server Components (RSC)

React 컴포넌트를 서버에서 렌더링하고, 그 결과만 클라이언트로 전송하는 기술입니다.

컴포넌트를 브라우저가 아닌 서버에서 실행해서 HTML을 생성하고, 그 결과만 클라이언트로 전달해서 **브라우저에서의 JavaScript 실행을 줄이는 방식**입니다.

> 클라이언트 JS 번들 크기 감소, 성능 최적화

---

App Router는 RSC를 기본값으로 사용합니다.

RSC 덕분에 클라이언트로 JS를 최소한만 전송해서 더 빠르고 최적화된 렌더링이 가능합니다.

> "use client"를 선언하여 클라이언트 컴포넌트로 전환 가능

---

## 예약어 파일

App Router 구조에서는 특정 **파일 이름들이 예약어(reserved filenames)**로 지정되어 있어서, 이 이름으로 파일을 만들면 Next.js가 특별한 방식으로 처리합니다.

- `layout.tsx`

  - 공통 레이아웃 (header, footer 등)
  - 하위 페이지에서 공유되며, 상태 유지 가능

- `page.tsx`

  - 실제 페이지(화면)
  - 해당 경로의 URL과 매칭 (/about/page.tsx -> /about)

- `loading.tsx`

  - 로딩 중 보여줄 UI
  - Suspense처럼 동작, 페이지/레이아웃 로딩 시 표시

- `not-found.tsx`

  - 404 페이지 UI
  - notFound() 호출 시 또는 매칭되는 경로가 없을 때 표시

- `error.tsx`

  - 해당 경로에서의 에러 처리 UI
  - try-catch처럼 작동하며 layout 아래에서만 사용 가능

- `global-error.tsx`

  - 앱 전체에서 발생한 에러 처리 UI
  - 최상위 /app 디렉터리에서만 사용

- `route.ts`

  - API 엔드포인트
  - GET, POST 등 메서드를 export하여 API 기능 구현

- `template.tsx`

  - 매번 새로 렌더링되는 레이아웃
  - layout과 비슷하지만 상태 공유 X (애니메이션 등에 사용)

- `default.tsx`

  - 병렬 라우트(parallel routes)의 fallback UI
  - 선택된 슬롯이 없을 때 보여짐

---

## 기본 폴더구조

App Router는 app 디렉토리 내에서 라우트 파일을 구성합니다.

각 파일은 기본적으로 URL 경로와 일치하며, **폴더 구조가 URL 경로를 결정**합니다.

```tsx
app/
├── layout.tsx          // 전체 레이아웃
├── page.tsx            // 홈 페이지
├── loading.tsx         // 홈 페이지 로딩 UI
├── error.tsx           // 홈 페이지 에러 UI
├── about/
│   ├── page.tsx        // /about 페이지
│   ├── not-found.tsx   // /about 전용 404 UI
├── blog/
│   └── [id]/
│       └── page.tsx    // /blog/:id 페이지
├── api/
│   └── hello/
│       └── route.ts    // /api/hello 엔드포인트
```

- `app/page.tsx` : `루트 경로 (/)`에 해당하는 페이지입니다.

- `app/about/page.tsx` : `/about` 경로에 해당하는 페이지입니다.

- `app/blog/[id]/page.tsx` : `/blog/:id`와 같은 동적 경로입니다.

---

## 기본 경로

`page.tsx`는 페이지를 정의하는 기본적인 파일입니다.

`page.tsx`는 루트 경로에 해당합니다.

```tsx
// app/page.tsx
import React from "react";

const Home: React.FC = () => {
  return <h1>Welcome to the Home Page!</h1>;
};

export default Home;
```

---

## 동적 경로

동적 경로는 대괄호(`[]`)를 사용하여 정의할 수 있습니다.

`app/blog/[id]/page.tsx`는 `blog/:id` 경로에 해당합니다.

```tsx
// app/blog/[id]/page.tsx
import React from "react";

interface BlogPostProps {
  params: {
    id: string;
  };
}

const BlogPost: React.FC<BlogPostProps> = ({ params }) => {
  return <h1>Blog Post ID: {params.id}</h1>;
};

export default BlogPost;
```

---

## 서버 측 데이터 가져오기

App Router에서는 **서버 컴포넌트**를 활용하여 데이터를 처리합니다.

```tsx
// app/blog/[id]/page.tsx
import { Suspense } from "react";
import axios from "axios";

// 비동기 함수로 블로그 글 데이터를 가져오기
const fetchBlogPost = async (id: string) => {
  try {
    const response = await axios.get(
      `https://jsonplaceholder.typicode.com/posts/${id}`
    );

    return response.data;
  } catch (error) {
    throw new Error("블로그 글을 가져오는 데 실패했습니다.");
  }
};

// 서버 컴포넌트: 특정 블로그 글을 표시하는 컴포넌트
const BlogPost: React.FC<{ id: string }> = async ({ id }) => {
  const blogPost = await fetchBlogPost(id); // 비동기 함수로 데이터를 가져옵니다.

  return (
    <div>
      <h1>{blogPost.title}</h1>
      <p>{blogPost.body}</p>
    </div>
  );
};

// Suspense로 로딩 상태와 에러 처리
const Page = ({ params }: { params: { id: string } }) => (
  <Suspense fallback={<div>로딩 중...</div>}>
    <BlogPost id={params.id} />
  </Suspense>
);

export default Page;
```

---

### 로딩 처리

App Router의의 `Suspense`를 사용해서 로딩 처리

```tsx
<Suspense fallback={<div>로딩 중...</div>}>
  <UserProfile />
</Suspense>
```

---

### 에러 처리

App Router의 `error.tsx` 사용해 에러 처리

```tsx
// app/blog/[id]/error.tsx
"use client";

export default function Error({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  return (
    <div>
      <h2>에러 발생: {error.message}</h2>
    </div>
  );
}
```
