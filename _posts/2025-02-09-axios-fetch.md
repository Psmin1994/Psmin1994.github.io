---
title: axios VS fetch
author: Psmin
data: 2025-02-09 15:12:06 +0900
categories: [Knowledge, NextJS]
tags: [axios, fetch]
---

# App Router에서 사용하는 axios와 fetch에 대해 알아보자.

---

## fetch

**App Router**의 서버 컴포넌트에서 `fetch`를 사용할 경우, 단순히 데이터를 가져오는 도구를 넘어서, Next.js의 렌더링 엔진과 깊게 통합된 고급 기능을 제공합니다.

---

**자동 캐싱**

Next.js는 기본적으로 응답을 SSG 방식처럼 캐싱합니다.

이로인해 동일한 요청 시, 사용자는 빠르게 결과를 받을 수 있습니다.

```tsx
// app/products/page.tsx
const getProducts = async () => {
  const res = await fetch("https://api.example.com/products"); // 자동 캐싱됨
  return res.json();
};

export default async function Page() {
  const products = await getProducts();

  return <div>{products.length}개의 상품</div>;
}
```

---

**캐시 무효화 (revalidate)**

`next: { revalidate: N }` 옵션을 사용해 일정 시간마다 캐시를 무효화하고 새 데이터를 가져오게 할 수 있습니다.

Next.js는 백그라운드에서 새로운 데이터를 요청해서 캐시를 갱신합니다.

> ISR 과 같은 효과

```tsx
const getPosts = async () => {
  const res = await fetch("https://api.example.com/posts", {
    next: { revalidate: 30 }, // 30초마다 새로고침
  });
  return res.json();
};
```

---

**Preloading**

Next.js가 빌드 시점에 필요한 데이터를 미리 요청해 사전 로딩합니다.

```tsx
// app/page.tsx
import Link from "next/link";

export default function Home() {
  return <Link href="/products">제품 목록 보러가기</Link>;
}

// app/products/page.tsx
const getProducts = async () => {
  const res = await fetch("https://api.example.com/products");
  return res.json();
};
```

`Link`에 **hover하는 순간 preload가 발생**해 사용자가 `/products`로 가기 전에, getProducts()의 fetch()를 미리 호출합니다.

> next/link를 사용할 때 기본으로 활성화

---

**병렬 요청 최적화**

하나의 서버 컴포넌트 안에서 여러개의 `fetch`를 요청하는 경우, 하나의 요청으로 통합되거나 병렬 처리합니다.

실제로는 `await` 없이 먼저 `fetch`를 실행해서 **Promise 객체를 저장**한 뒤, `Promise.all()` 등으로 병렬 처리합니다.

```tsx
const getUser = () =>
  fetch("https://api.example.com/user").then((res) => res.json());
const getOrders = () =>
  fetch("https://api.example.com/orders").then((res) => res.json());

export default async function Page() {
  const [user, orders] = await Promise.all([getUser(), getOrders()]); // 병렬 처리됨

  return (
    <div>
      <h1>{user.name}님의 주문 목록</h1>
      <ul>
        {orders.map((o: any) => (
          <li key={o.id}>{o.item}</li>
        ))}
      </ul>
    </div>
  );
}
```

---

**서버 전용 실행**

app/ 내부의 **서버 컴포넌트**에서만 실행되므로 클라이언트 번들에 포함되지 않습니다.

```tsx
// app/dashboard/page.tsx (서버 컴포넌트)
const getSecret = async () => {
  const res = await fetch(process.env.INTERNAL_API_URL!, {
    headers: {
      Authorization: `Bearer ${process.env.INTERNAL_TOKEN}`,
    },
  });
  return res.json();
};

export default async function DashboardPage() {
  const secret = await getSecret();
  return <div>민감 정보: {secret.data}</div>;
}
```

API Key, 토큰 등 보안 정보가 절대 클라이언트로 노출되지 않습니다.

---

**cache: 'no-store' 옵션**

SSR처럼 항상 새 데이터를 가져옵니다.

```tsx
// SSR: 항상 최신 데이터 (캐싱 없음)
fetch(url, { cache: "no-store" });
```
