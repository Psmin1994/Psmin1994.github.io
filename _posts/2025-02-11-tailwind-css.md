---
title: Tailwind CSS
author: Psmin
data: 2025-02-11 02:53:26 +0900
categories: [Knowledge, CSS]
tags: [Tailwind]
---

## Tailwind CSS

Tailwind는 유틸리티 퍼스트 (Utility-First) **CSS 프레임워크**로, 빠르게 UI를 구성할 수 있도록 도와줍니다.

> 유틸리티 퍼스트 (Utility-First)  
> : 작고 단일한 역할을 가진 CSS 클래스들을 조합해서 스타일링하는 방식

---

## 기본 구조

Tailwind는 **HTML 태그의 className 속성**에 클래스 이름만으로 스타일을 선언할 수 있도록 도와줍니다.

- `variants` + `property` + `value`

  - **variants** : 상태나 반응형 브레이크포인트를 나타냅니다. (예: hover:, sm:, focus:)
  - **property** : CSS 속성 (예: bg → background, text → 글꼴 크기, border → 테두리)
  - **value** : 속성 값 (예: blue-500, 4, lg)

---

## variants

variants는 스타일이 특정 조건에서만 적용되도록 만드는 **접두어(prefix)**입니다.

---

### 상태 기반 State-based

- `hover:` : 마우스 오버 시 적용
- `focus:` : 포커스(클릭 or Tab) 시 적용
- `active:` : 클릭 중일 때 적용
- `disabled:` : 비활성화된 요소에 적용
- `focus-visible:` : 키보드로 포커스가 갔을 때만 적용
- `group-hover:` : 부모 요소에 hover가 발생했을 때 자식에 적용

---

### 반응형 Responsive

- `sm:` : 작은 화면 이상 / `min-width: 640px`
- `md:` : 중간 화면 이상 / `min-width: 768px`
- `lg:` : 큰 화면 이상 / `min-width: 1024px`
- `xl:` : 더 큰 화면 이상 / `min-width: 1280px`
- `2xl:` : 매우 큰 화면 이상 / `min-width: 1536px`

---

### 기타

- `first:` : 첫 번째 자식 요소
- `last:` : 마지막 자식 요소
- `odd:` : 홀수 번째 자식
- `even:` : 짝수 번째 자식
- `only:` : 유일한 자식
- `empty:` : 자식이 없는 경우
- `nth-child(n):` : (플러그인 필요) n번째 자식

---

## 유틸리티 클래스

**property + value** 구조로 결합한 형태를 말합니다.

유틸리티 클래스들을 조합해서 **class 속성에 설정해주는 것으로 스타일링**이 가능하게 됩니다.

속성 별 사용 방법은 [공식 문서](https://tailwindcss.com/)에서 찾아볼 수 있습니다.

VSCode를 사용한다면면 `Tailwind CSS IntelliSense`, `Tailwind Docs` 같은 **Extensions**을 활용해도 좋습니다.

> Tailwind CSS IntelliSense : tailwindcss 자동완성  
> Tailwind Docs : 속성 검색 기능

---

### 레이아웃 Layout

- **display** : `block`, `inline-block`, `flex`, `grid`, `hidden`
- **position** : `relative`, `absolute`, `fixed`, `sticky`
- **top/right/bottom/left** : `top-0`, `left-4`, `bottom-2`, , `-bottom-2`
- **z-index** : `z-0`, `z-10`, `z-50`

---

### 박스 모델 Box Model

- **margin** : `m-4`, `mt-2`, `mx-auto`
- **padding** : `p-4`, `py-2`, `pl-3`

> `m`, `p` 뒤에 `t`(top), `r`(right), `b`(bottom), `l`(left), `x`(좌우), `y`(상하) 조합 가능

- **width**, **height** : `w-10`, `h-5`, `w-full`, `h-screen`
- **max-width**, **min-width** : `max-w-md`, `min-w-full`
- **border** : `border`, `border-2`, `border-red-500`
- **border-radius** : `rounded`, `rounded-md`, `rounded-full`

---

### 배경 Background

- **background-color** : `bg-blue-500`, `bg-gray-100`
- **background-image** : `bg-gradient-to-r`, `bg-none`
- **background-position** : `bg-center`, `bg-top`
- **background-size** : `bg-cover`, `bg-contain`

---

### 텍스트 Typography

- **font-size** : `text-sm`, `text-lg`, `text-2xl`
- **font-weight** : `font-bold`, `font-light`
- **text-align** : `text-left`, `text-center`
- **letter-spacing** : `tracking-wide`, `tracking-tight`
- **line-height** : ``leading-loose`, `leading-tight`
- **color** : `text-gray-500`, `text-white`
- `uppercase`, `lowercase`, `capitalize`

---

### Flex

- **flex** : `flex`, `inline-flex`, `flex-1`, `flex-none`
- **flex-direction** : `flex-row`, `flex-col`, `flex-row-reverse`
- **justify-content** : `justify-start`, `justify-between`, `justify-center`
- **align-items** : `items-start`, `items-center`, `items-end`
- **gap** : `gap-2`, `gap-x-4`, `gap-y-1`

---

### 효과 Effects

- **opacity** : `opacity-50`, `opacity-100`
- **transition** : `transition`, `duration-300`, `ease-in-out`
- **transform** : `transform`, `scale-110`, `rotate-45`, `translate-x-2`
