---
title: Link VS useNavigate
author: Psmin
data: 2023-04-21 17:53:16 +0900
categories: [Knowledge, ReactJS]
tags: [React, useNavigate]
---

## Link 컴포넌트

React에서 사용하는 `a 태그`와 유사하게 동작합니다.

a 태그는 전체 페이지를 재렌더링합니다.  
페이지 자체를 새로고침하므로 상태 값을 유지하지 못하며 속도도 저하됩니다.

Link 컴포넌트를 클릭하면 URL 경로가 바뀌면서 해당 경로로 지정된 컴포넌트만 재렌더링합니다.

- `<a>` : 외부 프로젝트에 연결에 사용
- `<Link>` : 프로젝트 내에서 페이지 전환에 사용

---

### Link 사용방법

```js
import { Link } from "react-router-dom";

<Link to="경로">내용</Link>;
```

react-router-dom 에서 Link를 import한 후 사용합니다.

to 속성에 이동할 경로를 입력합니다.

- 예시

  ```js
  import { Routes, Route, Link } from "react-router-dom";
  import Home from "./Home";
  import Login from "./Login";

  function App() {
    return (
      <div>
        <ul>
          <li>
            <Link to="/home">Home</Link>
          </li>
          <li>
            <Link to="/login">Login</Link>
          </li>
        </ul>
        <Routes>
          <Route path="/home" element={<Home />} />
          <Route path="/login" element={<Login />} />
        </Routes>
      </div>
    );
  }

  export default App;
  ```

  ![link](/assets/img/link.png){: .w-80}

  즉, 클릭 시 바로 이동하는 로직 구현 시에 사용합니다.

  - ex : 상품 리스트에서 상세 페이지 이동 시

---

## useNavigate

Link는 클릭 시 페이지를 이동하지만 useNavigate는 **_조건을 만족했을 때 페이지를 전환_**합니다.

useNavigate Hook은 페이지를 이동시킬 수 있는 함수를 반환합니다.

함수에 설정한 **_path 경로_**를 넘겨주면 해당 경로로 이동할 수 있습니다.

**_숫자 타입_**을 넘겨주면 뒤로 가거나 앞으로 이동할 수 있습니다.  
(navigate(-1) - 뒤로 가기 / navigate(1) - 앞으로 가기)

---

### useNavigate 사용방법

useNavigate 훅을 실행하면 페이지 이동을 할 수 있게 해주는 함수를 반환합니다.

반환받은 함수를 navigate라는 변수에 저장 후 navigate의 인자로 설정한 path값을 넘겨주면 해당 경로로 이동할 수 있습니다.

조건이 필요한 곳에서 navigate 함수를 호출해서 사용합니다.

- 예시

  로그인 페이지 에서 로그인 성공하면 메인페이지(/main)를 보여주고 실패하면 회원가입 페이지(/signup)를 보여주는 로직을 구현해보겠습니다.

  ```js
  import React from "react";
  import { useNavigate } from "react-router-dom";

  function Login() {
    // useNavigate 훅을 실행하면 페이지 이동을 할 수 있게 해주는 함수를 반환합니다.
    const navigate = useNavigate();

    const goToMain = () => {
      if (response.message === "login success") {
        // 로그인 성공
        navigate("/main");
      } else {
        // 로그인 실패
        alert("가입된 회원이 아닙니다.");
        navigate("/signup");
      }
    };

    return (
      <div>
        <button className="loginBtn" onClick={goToMain}>
          로그인
        </button>
      </div>
    );
  }

  export default Login;
  ```

  즉, 페이지 전환 시 추가로 처리해야 하는 로직이 있을 경우에 사용합니다.
