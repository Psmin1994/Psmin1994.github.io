---
title: Prisma 조건 및 필터링
author: Psmin
data: 2024-10-10 23:07:02 +0900
categories: [Knowledge, TypeScript]
tags: [prisma]
---

# Prisma에서 조건 및 필터링 옵션을 사용해보자.

Prisma에서 조건 및 필터링을 위한 옵션은 매우 유용하며, 데이터를 원하는 방식으로 정교하게 추출하거나 수정할 수 있습니다.

`where`, `select`, `include`, `orderBy`, `skip`, `take`는 쿼리에서 데이터를 필터링하고, 정렬하고, 필요한 데이터만 반환하도록 제어할 수 있는 옵션입니다.

---

## where

쿼리에서 조건을 설정하는 옵션으로, 데이터를 필터링하는 데 사용됩니다.

이를 통해 특정 필드를 기준으로 조건을 지정하고 원하는 레코드만 조회할 수 있습니다.

- **equals** : 정확히 일치하는 값
- **in** : 배열에 포함된 값 중 하나
- **notIn** : 배열에 포함되지 않은 값
- **lt** : 보다 작은 값
- **lte** : 보다 작거나 같은 값
- **gt** : 보다 큰 값
- **gte** : 보다 크거나 같은 값
- **not** : 특정 조건을 제외한 값
- **contains**, **startsWith**, **endsWith** : 문자열을 포함하거나 시작하거나 끝나는 값

```ts
const users = await prisma.user.findMany({
  where: {
    age: { gte: 18 }, // 18세 이상
    name: { contains: "Alice" }, // 이름에 'Alice' 포함
    email: { not: "bob@example.com" }, // 이메일이 'bob@example.com'이 아닌 사용자
  },
});
```

---

## select

반환할 필드를 선택하는 옵션입니다.

기본적으로 `findMany`나 `findUnique`는 모델의 모든 필드를 반환하지만, select를 사용하면 필요한 필드만 선택적으로 반환할 수 있습니다.

```ts
const user = await prisma.user.findUnique({
  where: {
    id: 1,
  },
  select: {
    id: true,
    name: true,
    posts: {
      select: {
        title: true, // 게시물의 title만 반환
      },
    },
  },
});
```

---

## include

관계형 데이터를 함께 조회할 때 사용됩니다.

예를 들어, `User` 모델이 `Post`와 관계가 있을 때, include를 사용하여 `User`와 관련된 ``Post 데이터를 함께 가져올 수 있습니다.

```ts
const user = await prisma.user.findUnique({
  where: {
    id: 1,
  },
  include: {
    posts: {
      include: {
        comments: true, // 중첩 가능
      },
    },
  },
});
```

---

## orderBy

조회한 데이터를 정렬할 때 사용됩니다.

정렬 기준을 하나 또는 여러 개 지정할 수 있습니다.

기본적으로 오름차순(asc)으로 정렬되며, 내림차순(desc)으로 정렬하려면 명시적으로 설정해야 합니다.

```ts
const users = await prisma.user.findMany({
  orderBy: [
    { age: "asc" }, // 나이 기준 오름차순
    { name: "desc" }, // 이름 기준 내림차순
  ],
});
```

---

## skip

쿼리 결과에서 처음 몇 개의 레코드를 건너뛰는 데 사용됩니다.

주로 페이징(pagination)에서 사용됩니다.

```ts
const users = await prisma.user.findMany({
  skip: 5, // 처음 5개의 레코드를 건너뛰고 조회
});
```

---

## take

쿼리 결과에서 반환할 최대 레코드 수를 지정하는 옵션입니다.

skip과 함께 사용하면 페이지 단위로 데이터를 가져올 수 있습니다.

```ts
const users = await prisma.user.findMany({
  skip: 10, // 첫 번째 페이지는 건너뛰고
  take: 10, // 두 번째 페이지의 10개 사용자 조회
});
```
