---
title: React의 Attributes
author: Psmin
data: 2023-03-09 11:23:33 +0900
categories: [Knowledge, ReactJS]
tags: [React Attribute]
---

## DOM Element의 Attributes

1. 기본적인 DOM Element(div, span 등)은 ‘data-’ 또는 ‘aria-’ 로 시작하는 Attribute를 제외하고는 camel case로 작성합니다.
   (tabIndex, className 등)

2. HTML의 Attribute와 다른 이름을 가지는 Attribute가 있습니다.  
   (class => className, for => htmlFor 등)

3. HTML의 Attribute와 다른 동작 방식을 가진 Attribute가 있습니다.  
   (checked => defaultChecked, value => defaultValue 등)

4. React에서만 쓰이는 Attribute가 있습니다.  
   (key, dangerouslySetInnerHTML)

---

## defaultChecked, defaultValue

```js
<input type="checkbox" checked={false} />
```

HTML 에서 `checked`, `value`는 초기 값으로 사용하지만 React에서는 `현재 값을 의미`합니다.

즉, 위 코드는 checked 값(현재 값)을 false로 고정해두었기때문에 사용자가 체크박스를 클릭해도 변하지 않습니다.

React 에서 초기 값을 사용하려면 `defaultChecked`, `defaultValue`를 사용해야합니다.

---

## key

key는 React가 어떤 항목을 변경, 추가 또는 삭제할지 식별하는 것을 도와줍니다.

key는 다른 항목들 사이에서 고유하게 식별할 수 있는 것을 사용합니다.  
대부분의 경우는 data.id 값으로 key로 사용합니다.

key는 형제 사이에서만 고유한 값이어야 합니다.  
두 개의 다른 배열을 만들 때는 동일한 key를 사용할 수 있습니다.
