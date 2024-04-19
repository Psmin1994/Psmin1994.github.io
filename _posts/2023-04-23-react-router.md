---
title: react-router-dom
author: Psmin
data: 2023-04-23 23:47:22 +0900
categories: [Knowledge, ReactJS]
tags: [React, Router]
---

## React Routing

React는 웹 사이트의 전체 페이지를 **하나의 페이지로 담아 동적으로 화면을 변환**해가며 보여줍니다.

이러한 방식을 `SPA (Single Page Application)`이라고 합니다.

SPA에서는 사용자에게 브라우저의 주소(URL)에 따른 다양한 화면을 보여줄 수 있습니다.

<u>URL에 따라서 그에 상응하는 다양한 화면을 보여주는 것</u>을 **Routing**이라고 합니다.

React에서 Routing을 구현하기위해 여러 라이브러리들을 사용합니다.

이번 포스트에서는 **react-router-dom 라이브러리**를 알아보겠습니다.

---

## react-router-dom

**react-router**는 React에서 Routing을 구현하기위해 가장 많이 사용되는 라이브러리입니다.

v4 이후로 react-router를 베이스로 **웹 개발을 위한 react-router-dom**과 **앱 개발을 위한 react-router-native**가 생겨났습니다.

react-router-dom은 react-router 모듈에 dom이 바인딩 되어 있는 모듈입니다.

- 설치
  ```
  npm i react-router-dom
  ```

---

## react-router-dom 내장 컴포넌트

- **BrowserRouter**

  HTML5의 **history API를 이용해 UI 업데이트**를 합니다.

- **HashRouter**

  #가 들어간 해시 주소(http://example.com/#test)를 감지합니다.

  > 검색 엔진(SEO)가 감지하지 못하는 치명적인 단점이 있어 거의 사용되지 않습니다.

- **Routes**

  Route로 생성된 자식컴포넌트 중에 **매칭되는 첫번째 Route**를 렌더링 해줍니다.

  이것을 이용해 특정 컴포넌트만 화면에 띄울 수 있습니다.

- **Route**

  페이지의 경로와 **렌더링할 페이지를 정의**하는 컴포넌트입니다.

- **Link**

  지정한 **URL로 이동**되게 해줍니다.

  아예 새로운 페이지를 불러오므로 기존 컴포넌트의 상태값은 소멸됩니다.

---

## 사용해보기

가장 기본적인 사용방법을 알아보겠습니다.

1. Routing할 컴포넌트들을 **BrowserRouter 컴포넌트**로 감싸줍니다.

2. **Route**로 컴포넌트별로 <u>path에 원하는 URL 주소를 할당</u>하고 <u>element에 URL별로 가져올 컴포넌트를 할당</u>합니다.

3. 할당된 `Route`들을 `Routes`로 감싸줍니다.

```js
//App.js
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Header from "./pages/Header";
import Home from "./pages/Home";
import Profile from "./pages/Profile";
import Board from "./pagres/Board";

function App() {
  return (
    <BrowserRouter>
      <div className="App">
        <Header />
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/profile" element={<Profile />} />
          <Route path="/board" element={<Board />} />
        </Routes>
      </div>
    </BrowserRouter>
  );
}
```

---

## Link

페이지를 **갱신하지 않고 렌더링 방식으로 이동**하려면 a 태그 대신 Link를 써야합니다.

이동할 페이지를 **to**에 할당합니다.

```js
import { Link } from "react-router-dom";

function Nav() {
  return (
    <div>
      <Link to="/">Home</Link>
      <Link to="/board">Board</Link>
      <Link to="/profile">Profile</Link>
    </div>
  );
}

export default Nav;
```

---

## react-router-dom의 여러 Hook

- **useParams()**

  URL을 통해서 넘겨받은 Parameter 값을 사용합니다.

  path 경로 뒤에 <kbd>/:</kbd>에 값을 넣은 **path Variable을 받아서 사용**합니다.

  ```js
  // App.jsx

  import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
  import "./App.css";
  import Detail from "./routes/Detail";

  function App() {
    return (
      <Router>
        <Routes>
          <Route path="/detail/:id" element={<Detail />} />
        </Routes>
      </Router>
    );
  }

  export default App;

  // Detail.jsx
  import { useContext } from "react";
  import { useParams } from "react-router-dom";

  export default function Detail() {
    // useParams로 path 경로에서 :/로 받은 값을 객체로 받습니다.
    const {id} = useParams();
    const context = useContext(Context);

    return (
      <section>
        <h1>id 값은 {id}</h1>
      </section>
    )
  }
  ```

- **useSearchParams()**

  URL을 통해서 넘겨받은 **Query String** 값을 사용합니다.

  path 경로 뒤에 <kbd>?</kbd>에 값을 넣으며 <kbd>&</kbd>로 구분해서 사용합니다.

  주로 useState처럼 배열의 비구조화 할당을 통해 값을 받습니다.

  - **searchParams** : 데이터의 값
  - **setSearchParams** : 데이터를 변경시킬 수 있는 함수

  - **여러 메서드**

    - _searchParams.get(key)_ : 입력한 key의 value를 하나 가져오는 메서드

    - _searchParams.getAll(key)_ : 입력 key 에 해당하는 모든 value 를 가져오는 메서드

    - _searchParams.toString()_ : 쿼리 스트링을 string 형태로 리턴

    - _searchParams.set(key, value)_ : 인자로 전달한 key 값을 value 로 설정, 기존에 값이 존재했다면 그 값은 삭제

    - _searchParams.append(key, value)_ : 기존 값을 변경하거나 삭제하지 않고 추가합니다.

> useState처럼 set함수로 변경할 수도 있지만 객체 전체를 입력해야합니다.

```js
setSearchParams({ id: 5, key }); // id 쿼리스트링 업데이트
setSearchParams({ id, key: 2323 }); // key 쿼리스트링 업데이트
```

```js
// localhost:3000/test?id=10&mode=dark
import { useSearchParams } from "react-router-dom";

const Edit = () => {
  const [searchParams, setSearchParams] = useSearchParams();

  const id = searchParams.get("id"); // 10
  const mode = searchParams.get("mode"); // dark

  console.log("id :", id, "/ mode : ", mode);
};
```

- **useNavigate()**

  페이지를 강제로 이동시키는 합수입니다.

  - 직접 URL을 넣어 페이지 이동
  - 숫자를 넣으면 그만큼 뒤로가기 / 앞으로 가기
  - 객체 형식으로 URL과 Query를 조합할 수 있습니다.  
    (Query String은 react-router-dom의 **createSearchParams()**로 만듭니다.)

  ```js
  import { useNavigate, createSearchParams } from "react-router-dom";

  const navigate = useNavigate();

  navigate("/login"); // 로그인 페이지로 이동

  navigate(-1); // 한 단계 전으로 (브라우저의 뒤로가기 한번)
  navigate(2); // 두 단계 앞으로 (브라우저의 앞으로가기 두번)

  navigate({
    // /book?sort=asc&page=3 으로 이동
    pathname: "/book", // useLocation()의 pathname을 넣을 수도 있다.
    search: `?${createSearchParams({
      sort: "asc",
      page: 3,
    })}`,
  });
  ```

- **useLocation()**

  현재 URL의 정보를 객체 형태로 받습니다.

  - **pathname** : 현재 URL
  - **search** : Query 전체 값

  ```js
  import { useLocation } from "react-router-dom";

  const location = useLocation();

  // 현재 url -> /book/11?page=3&sort=asc
  const { pathname, search } = location;
  // pathname -> /book/11
  // search -> ?page=3&sort=asc
  ```
