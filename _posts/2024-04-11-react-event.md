---
title: e.preventDefault()와 e.stopPropagation()의 차이점
author: Psmin
data: 2024-04-11 15:31:13 +0900
categories: [Knowledge, ReactJS]
tags: [Event]
---

# e.preventDefault() VS e.stopPropagation()

두 메서드는 **이벤트 리스너**에서 이벤트 중단에 사용합니다.

비슷한 역할을 하는 두 메서드의 차이점에 대해 정리해보겠습니다.

---

## e.preventDefault()

현재 이벤트의 **기본 동작을 중단**하는 메서드입니다.

체크 박스의 체크 , 입력 칸에 값 채우기, a 태그의 URL 이동 등 기본 동작을 중단할 수 있습니다.

```jsx
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log("The link was clicked.");
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

---

## e.stopPropagation()

상위 DOM으로 **이벤트 전파를 중단**합니다.

즉, **최하위 태그의 Click 이벤트**에 e.stopPropagation()을 추가하면 상위 DOM으로 **이벤트가 전파되지 않아서 해당 태그의 이벤트만 동작**하는 방식입니다.

```jsx
function ActionLink() {
  function handleClick(e) {
    console.log("This is Top.");
  }
  function handleClick(e) {
    console.log("This is Middle.");
  }
  function handleClick(e) {
    e.stopPropagation();
    console.log("This is Bottom.");
  }

  return (
    <div id="top">
      최상위 태그
      <p id="middle">
        중간 태그
        <span id="bottom" onClick={handleClick}>
          최하위 태그
        </span>
      </p>
    </div>
  );
}
```

최하위 태그 클릭시 결과

```
This is Bottom.
```
