---
title: Skeleton UI
author: Psmin
data: 2025-02-09 02:13:02 +0900
categories: [Knowledge, NestJS]
tags: [Suspense, Skeleton UI]
---

## Skeleton UI

Skeleton UI는 로딩 중일 때 보여주는 **실제 컨텐츠의 윤곽선이나 형태**를 흉내 낸 UI를 말합니다.

사용자에게 **“여기에 뭔가 뜰 거구나!”** 하는 기대감을 줄 수 있습니다.

로딩이 빨라진 건 아니지만, **지각상 속도(perceived speed)**를 높여줍니다.

---

## 기본 예시

```tsx
// app/page.tsx
import { Suspense } from "react";
import UserProfile from "./UserProfile";
import UserProfileSkeleton from "./components/UserProfileSkeleton";

export default function Page() {
  return (
    <div className="p-6">
      <h1 className="text-xl mb-4">사용자 정보</h1>

      <Suspense fallback={<UserProfileSkeleton />}>
        <UserProfile />
      </Suspense>
    </div>
  );
}
```

```tsx
// app/components/UserProfileSkeleton.tsx
export default function UserProfileSkeleton() {
  return (
    <div className="animate-pulse space-y-4">
      <div className="w-16 h-16 bg-gray-300 rounded-full" />
      <div className="h-4 w-32 bg-gray-300 rounded" />
      <div className="h-3 w-48 bg-gray-200 rounded" />
    </div>
  );
}
```
