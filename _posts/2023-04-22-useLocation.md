---
title: useLocation
author: Psmin
data: 2023-04-22 13:12:43 +0900
categories: [Knowledge, ReactJS]
tags: [React, useLocation]
---

## useLocation

> The useLocation hook returns the location object that represents the current URL. You can think about it like a useState that returns a new location whenever the URL changes.

useLocation 훅은 **현재의 URL을 대표하는 location 객체를 반환**합니다.

URL이 바뀔 때마다 새로운 location이 반환되는 useState 처럼 생각할 수 있습니다.

useLocation이 반환하는 Location 객체를 확인해보겠습니다.

```js
import React from "react";
import {
  BrowserRouter as Router,
  Switch,
  Route,
  Link,
  useLocation,
} from "react-router-dom";

export default function BasicExample() {
  return (
    <Router>
      <div>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/about">About</Link>
          </li>
        </ul>
        <hr />
        <Switch>
          <Route exact path="/">
            <Home />
          </Route>
          <Route path="/about">
            <About />
          </Route>
        </Switch>
      </div>
    </Router>
  );
}

function Home() {
  return (
    <>
      <p>Home</p>
    </>
  );
}

function About() {
  let locationObj = useLocation();

  console.log(locationObj);

  return (
    <>
      <p>About</p>
    </>
  );
}
```

About링크를 누르면 locationObj에 location객체가 저장되고 locationObj가 콘솔에 출력됩니다.

![location-obj](/assets/img/location-obj.png){: }

- **pathname** : path경로 문자열
- **search** : ?부터 나오는 문자열 전부
- **hash** : #부터 나오는 문자열 전부
- **key** : 랜덤의 6글자 문자열

  > history stack에서 이 해당하는 location 객체를 찾기위한 고유의 문자열 키입니다.

- **state** : URL에 붙여서 보내는 정보가 아닌 숨겨서 또는 코드로 보내는 정보

  > URL처럼 공개적으로 보여지면 안되거나 URL 글자 수 제한에 영향을 주는 경우 state로 보냅니다.

---

## useNavigate()와 useLocation()으로 데이터 주고받기

**useNavigate Hook**을 사용할 때 **2번째 인자로 location 객체의 state로 데이터**를 보낼 수 있습니다.

- **두번쨰 인자의 state 속성**으로 데이터를 담아서 보냅니다.

  ```js
  import { useNavigate } from "react-router-dom";

  // ...생략

  const navigate = useNavigate();

  // ...생략

  const gotoMain = () => {
    navigate("/main", {
      state: {
        userId: user.uid,
      },
    });
  };
  ```

- location 객체의 state에 내가 보낸 데이터들이 담겨있습니다.

  ```js
  import { useLocation } from "react-router-dom";

  // ...생략

  const location = useLocation();

  // ...생략

  const [userId, setUserId] = useState(location.state.userId);
  ```

- Link를 통해서도 데이터를 보낼 수 있습니다.

  ```js
  <Link to={`/main`} state={{ test: "hello world" }}>
    test
  </Link>
  ```
