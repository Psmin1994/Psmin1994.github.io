---
title: Conditional Rendering
author: Psmin
data: 2023-03-30 16:36:12 +0900
categories: [Knowledge, ReactJS]
tags: [React, Rendering]
---

## Javascript의 Truthy / Falsy

- **Falsy**

  boolean을 기대하는 문맥에서 false로 평가되는 값입니다.

  `0`, `-0`, `0n(bigint)`, `''(빈 문자열)`, `null`, `undefined`, `NaN(Not a Number)`

- **Truthy**

  boolean을 기대하는 문맥에서 true로 평가되는 값입니다.

  Falsy한 값을 제외한 모든 값입니다.

---

## 조건부 렌더링 Conditional Rendering

> 조건부 렌더링이란?  
> React에서는 원하는 동작을 캡슐화하는 컴포넌트를 만들 수 있습니다.  
> 이렇게 하면 애플리케이션의 상태에 따라서 컴포넌트 중 몇 개만을 렌더링할 수 있습니다. (출처 - React 공식 문서)

쉽게 말하자만 원하는 조건에 따라 다른 결과를 렌더링 하는 것입니다.

if 같은 조건문을 사용하여 원하는 어떤 컴포넌트를 렌더링할지 선택할 수 있도록 하는 것입니다.

---

## Element 변수

React의 **Element를 변수처럼** 다루는 방법입니다.

변수를 사용해 출력의 다른 부분은 변하지 않은 상태로 컴포넌트의 일부를 조건부로 렌더링 할 수 있습니다.

```js
import { useState } from "react";

function LoginControl(props) {
  const [isLoggedIn, setInLoggedIn] = useState(false);

  const handleLoginClick = () => {
    setIsLoggedIn(true);
  };

  const handleLogoutClick = () => {
    setIsLoggedIn(false);
  };

  let button;

  // isLoggedIn의 값에 따라 button 변수에 컴포넌트를 대입합니다.
  // 실제로는 컴포넌트가 생성한 Element가 button에 담깁니다.
  if (isLoggedIn) {
    button = <LogoutButton onClick={handleLogoutClick} />;
  } else {
    button = <LoginButton onClick={handleLoginClick} />;
  }

  return (
    <div>
      <Greeting isLoggedIn={isLoggedIn}>
      {button}
    </div>
  )
}

function LoginButton(props) {
  return <button onClick={props.onClick}>로그인</button>;
}

function LogoutButton(props) {
  return <button onClick={props.onClick}>로그아웃</button>;
}
```

---

## 논리연산자 && 로 if를 inline 으로 표현하기

JSX 안에서는 {} (중괄호)를 활용하여 JavaScript 표현식을 넣을 수 있다는 특징을 활용하여 &&를 활용할 수 있습니다.

&& 연산자는 왼쪽에서 평가된 값이 '참' 인 경우 오른쪽의 값으로 평가되고, '거짓' 인 경우엔 렌더링 하지 않습니다.

> JavaScript 에서 true && expression은 항상 expression, `false && expression은 항상 false가 됩니다.

```js
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      {unreadMessages.length > 0 && (
        <h2>You have {unreadMessages.length} unread messages.</h2>
      )}
    </div>
  );
}
```

여기서 위에서말한 **0과 같은 falsy 표현식은 false를 반환**할 수 있으니 주의해야합니다.

---

## 삼항 연산자로 if-else구문 inline 으로 표현하기

삼항 연산자는 **condition ? true: false 형태**로 true에 해당하는 부분은 조건이 참일 때 렌더링하고 false에 해당하는 부분은 거짓일 때 렌더링합니다.

만약 아무것도 렌더링하고 싶지 않을 때는 null을 반환하면 됩니다.

```js
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;

  return (
    <div>
    {isLoggedIn ? <UserGreeting /> : <GuestGreeting />}
    </div>;
  )
}
```

---

## switch, enum 활용

여러 케이스를 렌더링을 할 때는 **switch 를 활용하여 case 에 따라 다른 값을 return** 하는 방식으로 사용합니다.

```js
function Fruit(props) {
  const fruit = props.fruit;

  switch (fruit) {
    case "oranges":
      return <Oranges />;
    case "apples":
      return <Apples />;
    default:
      return null;
  }
}
```

enum을 활용하면 좀 더 간결하게 표현할 수 있습니다.

```js
const FruitType = {
  oranges: <Oranges />,
  apples: <Apples />,
};

function Fruit(props) {
  const fruit = props.fruit;

  return <>{FruitType[fruit]}</>;
}
```
