---
title: Grid (Tailwind CSS)
author: Psmin
data: 2025-02-11 16:21:43 +0900
categories: [Knowledge, CSS]
tags: [Tailwind, Grid]
---

## Tailwind CSS를 사용해 Grid 시스템을 적용해보자.

---

## 열/행 개수 지정

- **grid-cols-n** : 가로로 item들을 n개씩 배치합니다.

  ![Tailwind-grid-col](/assets/img/tailwind-grid-col.png)

- **grid-rows-n** : 세로로 item들을 n개씩 배치합니다.

  ![Tailwind-grid-row](/assets/img/tailwind-grid-row.png)

---

## 셀 병합

- **col-span-n** : 요소가 가로 n개의 item칸 만큼 차지합니다.

- **row-span-n** : 요소가 세로 n개의 item칸 만큼 차지합니다.

---

## 그 외 클래스

- **place-items-center** : 내부 요소 정렬

- **grid-cols-\[...\]** : 커스텀 값 적용

---

## 반응형 디자인

반응형 variants를 사용해 반응형 Grid를 구현할 수 있습니다.

```html
<div
  class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6"
>
  <!-- 아이템들 -->
</div>
```

---

## 동적 Grid

`auto-fit` + `minmax` 조합으로 레이아웃 구성 가능

```html
<div class="grid grid-cols-[repeat(auto-fit,minmax(200px,1fr))] gap-4">
  <!-- 콘텐츠 -->
</div>
```
