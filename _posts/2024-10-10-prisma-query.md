---
title: Prisma 쿼리 메서드
author: Psmin
data: 2024-10-10 22:21:31 +0900
categories: [Knowledge, TypeScript]
tags: [prisma]
---

# Prisma에서 쿼리 메서드를 사용해보자.

Prisma에서 제공하는 기본 CRUD 쿼리 메서드는 데이터베이스와 상호작용할 때 가장 많이 사용되는 메서드들입니다.

메서드를 사용하여 데이터를 생성(CREATE), 조회(READ), 수정(UPDATE), **삭제(DELETE)**할 수 있습니다.

---

## create

새로운 레코드를 데이터베이스에 생성할 때 사용됩니다.

`data` 객체를 통해 새로운 레코드를 정의하고 이를 생성합니다.

```ts
const user = await prisma.user.create({
  data: {
    name: "Alice",
    email: "alice@example.com",
  },
});
```

---

## findUnique

단일 레코드를 고유한 값(주로 id나 유니크 필드)을 기준으로 조회할 때 사용됩니다.

`where` 조건을 통해 고유한 값으로 하나의 레코드를 찾아옵니다.

```ts
const user = await prisma.user.findUnique({
  where: {
    id: 1, // 유니크한 조건으로 조회
  },
});
```

---

## findFirst

조건에 맞는 첫 번째 레코드를 조회하는 메서드입니다.

```ts
const user = await prisma.user.findFirst({
  where: {
    name: "Alice", // 이름이 'Alice'인 첫 번째 사용자 조회
  },
});
```

---

## findMany

여러 레코드를 조회할 때 사용됩니다.

```ts
const users = await prisma.user.findMany({
  where: {
    active: true, // 'active' 상태인 사용자만 조회
  },
  orderBy: {
    name: "asc", // 이름 기준으로 오름차순 정렬
  },
  take: 10, // 최대 10개 사용자만 조회
});
```

---

## update

기존 레코드를 수정할 때 사용됩니다.

`where` 조건으로 업데이트할 레코드를 찾고, `data` 객체를 통해 수정할 필드를 설정합니다.

```ts
const updatedPost = await prisma.post.update({
  where: {
    id: 1, // 포스트 ID로 조회
  },
  data: {
    title: "Updated title", // 제목 업데이트
  },
});
```

---

## updateMany

여러 레코드를 한 번에 수정할 때 사용됩니다.

```ts
const updatedUsers = await prisma.user.updateMany({
  where: {
    age: { gte: 18 }, // 나이가 18세 이상인 사용자들
  },
  data: {
    status: "verified", // 'verified' 상태로 업데이트
  },
});
```

---

## delete

단일 레코드를 삭제할 때 사용됩니다.

```ts
const deletedUser = await prisma.user.delete({
  where: {
    id: 1, // 삭제할 사용자 ID
  },
});
```

---

## deleteMany

여러 레코드를 한 번에 삭제할 때 사용됩니다.

조건에 맞는 모든 레코드를 삭제할 수 있습니다.

```ts
const deletedPosts = await prisma.post.deleteMany({
  where: {
    createdAt: { lt: new Date("2023-01-01") }, // 2023년 1월 1일 이전에 작성된 포스트 삭제
  },
});
```
