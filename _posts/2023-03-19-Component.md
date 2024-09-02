---
title: Component와 Props
author: Psmin
data: 2023-03-19 11:23:33 +0900
categories: [Knowledge, ReactJS]
tags: [React, Props, Component]
---

## React Component

개념적으로 컴포넌트는 JavaScript 함수와 유사합니다.

**props**라고 하는 임의의 입력을 받은 후, 화면에 어떻게 표시되는지를 기술하는 **React Element를 반환**합니다.

여기서 중요한 특징은 **모든 React Element는 props를 직접 변경할 수 없으며, 같은 props에 대해서는 항상 같은 결과를 반환**합니다.

- **Function Component**

  ```js
  function Hello(props) {
    return <h1> Hello, {props.name} </h1>;
  }
  ```

  Hello 함수는 props를 받아 Element를 반환합니다.

- **Class Component**

  ES6 Class를 이용해서도 Component를 정의할 수 있습니다.

  ```js
  class Hello extends React.Component {
    render() {
      return <h1>Hello, {this.props.name}</h1>;
    }
  }
  ```

React의 관점에서 두 가지 유형의 컴포넌트는 동일합니다.

---

## Props 속성

**어떠한 값을 Component에 전달**해줄 때, props를 사용합니다.

App 컴포넌트에서 Hello 컴포넌트로 name 값을 전달해주고싶을 때, props를 이용합니다.

props는 **객체 형태로 전달**되며, **props.name 형태로 조회**합니다.

또한, **읽기 전용 Read-Only**으로 값을 변경할 수 없습니다.

정리하면 props는 **컴포넌트에 전달할 다양한 정보를 담고 있는 자바스크립트 객체**입니다.

---

## props 전달 방법

- props 1개

  ```js
  // App.js
  import React from 'react';
  import Hello from './Hello';

  function App() {
    return (
      <Hello name="react" />
    );
  }

  export default App;

  // Hello.js
  import React from 'react';

  function Hello(props) {
    return <div>Hello, {props.name}</div>
  }

  export default Hello;
  ```

- 여러 개의 props

  ```js
  // App.js
  import React from 'react';
  import Hello from './Hello';

  function App() {
    return (
      <Hello
        name="react"
        color="red"
      />
    );
  }

  export default App;

  // Hello.js
  import React from 'react';

  function Hello(props) {
    return <div style={{ color: props.color }}>Hello, {props.name}</div>
  }

  export default Hello;
  ```

- 여러개의 props, 비구조화 할당

  ```js
  // App.js
  import React from 'react';
  import Hello from './Hello';

  function App() {
    return (
      <Hello name="react" color="red"/>
    );
  }

  export default App;

  // Hello.js
  import React from 'react';

  function Hello({ color, name }) {
    return <div style={{ color }}>Hello, {name}</div>
  }

  export default Hello;
  ```

---

## Component 렌더링

**Component는 클래스, Element는 해당 클래스로 찍어낸 인스턴스**라고 볼 수 있습니다.

즉, 화면에 렌더링 되는 것은 Component가 아닌 **Component로 찍어낸 Element들이 렌더링 되는 것**입니다.

- **Componet로 Element 생성하기**

  ```js
  function Hello(props) {
    return <h1> Hello, {props.name} </h1>;
  }

  const element = <Hello name="Psmin" />;

  ReactDOM.render(element, document.getElementById("root"));
  ```

{: .prompt-info}

> Component의 이름은 항상 대문자로 시작해야합니다.  
> React는 소문자로 시작하는 Component를 DOM 태그로 처리합니다.

---

## Component 합성

여러 개의 **Component를 합쳐서 하나의 Component를 만드는 것**을 말합니다.

React는 컴포넌트 안에 또 다른 컴포넌트를 쓸 수 있습니다.

이러한 특징으로 **복잡한 화면을 여러 개의 컴포넌트로 나누어 구현** 가능합니다.

![Components](/assets/img/component-merge.png){: .normal}

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App(props) {
  return (
    <div>
      <Welcome name="Mike" />
      <Welcome name="Steve" />
      <Welcome name="Jane" />
    </div>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```

props의 값을 다르게 해서 **Hello 컴포넌트를 여러번 사용해 하나의 App 컴포넌트를 구성**할 수 있습니다.

---

## Component 추출

**Component에서 일부를 추출해 새로운 Component를 생성**합니다.

Component는 핵심 Component를 추출할수록 간단해집니다.

React에서는 **컴포넌트의 재사용을 위해 컴포넌트를 적절히 합성하고 추출**하는 것이 좋습니다.
