---
title: px과 rem
author: Psmin
data: 2025-02-11 17:33:26 +0900
categories: [Knowledge, CSS]
tags: [Tailwind]
---

## px 사용

Tailwind CSS는 기본적으로 rem 단위를 사용하여 반응형 디자인을 고려한 크기 조정에 유리하지만, 특정 상황에서는 px 단위를 사용하여 더 세밀한 조정이 필요할 수 있습니다.

예를 들어, `min-w-[320px]`와 같은 방식으로 px 단위를 사용하면 정확한 픽셀 크기를 지정할 수 있습니다.

---

## px -> rem 변환 이유

- **접근성 향상**

  사용자가 브라우저 기본 폰트 크기를 키우면, rem 단위는 같이 커지기 때문에 시각적 접근성이 좋아짐

- **반응형 디자인 유연성**

  rem은 루트 폰트 크기에 따라 전체 UI가 비례적으로 조절되어 반응형 구현에 유리함

- **디자인 일관성 유지**

  폰트, 간격 등을 상대 단위로 구성하면 전체 디자인 시스템 관리가 편함

---

## tailwindcss-preset-px-to-rem

px 단위를 rem 단위로 자동 변환하는 도구로 프로젝트 내에서 px 단위를 rem 단위로 일관되게 변환하여 사용하도록 도와줍니다.

---

### 문제점

`tailwindcss-preset-px-to-rem`을 사용할 경우, `width`, `height`, `min/max-width` 같은 속성은 조심해서 사용해야 합니다.

사용자가 브라우저 기본 폰트 크기(16px)를 변경했을 때 width도 비례해서 바뀌게 되어어 오히려 문제가 생길 수 있습니다.

---

## 추천 전략

- `Typography`, `spacing`, `padding/margin` → rem 사용

  폰트 크기, 여백은 상대 크기 기반이 접근성과 반응형에 유리

- `width`, `height`, `layout size`, `border-radius` → px 유지

  레이아웃 안정성과 정밀한 제어를 위해 픽셀 단위가 안전

---

## 결론

Tailwind CSS를 사용해 rem 사용할 항목은 Tailwind 기본 유틸리티 유지하고 px 단위가 필요한 항목은 [] 유틸리티로 명시해서 사용하자.

```tsx
<div class="text-sm leading-snug p-4 gap-2">
  <p class="text-base">타이포그래피</p>
</div>

<div class="w-[320px] h-[180px] rounded-[8px] shadow-[0_4px_12px_rgba(0,0,0,0.1)]">
  고정 크기 박스
</div>
```
