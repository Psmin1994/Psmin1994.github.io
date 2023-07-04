---
title: Composition
author: Psmin
data: 2023-03-15 21:35:05 +0900
categories: [Knowledge, ReactJS]
tags: [React, Composition]
---

## Composition 합성

React에서 컴포넌트를 구성하는 방법으로 여러 개의 컴포넌트를 합쳐서 새로운 컴포넌트를 만드는 것을 말합니다.

그렇다면 여러 개의 컴포넌트들을 어떻게 조합할까요?

---

## Containment

하위 컴포넌트를 포함하는 형태의 합성 방법을 말합니다.

Sidebar, Dialog 같은 Box 형태의 컴포넌트는 자신의 하위 컴포넌트를 미리 알 수 없습니다.

이러한 경우 Containment 방법을 사용하여 합성합니다.

React 컴포넌트 Props에 기본적으로 내장된 children을 사용합니다.

```js
function FancyBorder(props) {
  return (
    <div className={"FancyBorder FancyBorder-" + props.color}>
      {props.children}
    </div>
  );
}
```

여기서 children은 React의 createElement의 세번째 매개변수를 말합니다.

```js
React.createElement(type, [props], [...children]);
```

`FancyBorder` 컴포넌트를 실제로 사용해보겠습니다.

```js
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">Welcome</h1>
      <p className="Dialog-message">Thank you for visiting our spacecraft!</p>
    </FancyBorder>
  );
}
```

FancyBorder 컴포넌트 안에 있는 모든 JSX 태그(h1, p)는 children으로 전달됩니다.

> 만약 여러 개의 children 집합이 필요한 경우는 어떻게 할까요?

이런 경우에는 children 대신 자신만의 고유한 방식을 적용할 수도 있습니다.

즉, 고유의 props로 React Element를 보내줍니다.

```js
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">{props.left}</div>
      <div className="SplitPane-right">{props.right}</div>
    </div>
  );
}

function App() {
  return <SplitPane left={<Contacts />} right={<Chat />} />;
}
```

`<Contacts />`와 `<Chat />`같은 React 엘리먼트는 단지 객체이기 때문에 다른 데이터처럼 prop으로 전달할 수 있습니다.

---

## Specialization 특수화

어떤 컴포넌트의 “특수한 경우”인 컴포넌트를 고려해야 하는 경우가 있습니다.

예를 들어, WelcomeDialog는 Dialog의 특수한 경우라고 할 수 있습니다.

React에서는 이 역시 합성을 통해 해결할 수 있습니다.

더 “구체적인” 컴포넌트가 “일반적인” 컴포넌트를 렌더링하고 props를 통해 내용을 구성합니다.

```js
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">{props.title}</h1>
      <p className="Dialog-message">{props.message}</p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog title="Welcome" message="Thank you for visiting our spacecraft!" />
  );
}
```

위 코드는 더 구체적인 컴포넌트인 WelcomeDialog에서 Dialog에게 title, message props로 화면에 출력 될 값을 넣어주고, Dialog에서는 WelcomeDialog로부터 받은 props로 화면을 구성합니다.

즉, Specialization `범용적인 개념을 구별 되게 구체화 하는 것`을 말합니다.

---

## Containment와 Specialization 같이 사용하기

```js
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">{props.title}</h1>
      <p className="Dialog-message">{props.message}</p>
      {props.children}
    </FancyBorder>
  );
}

class SignUpDialog extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSignUp = this.handleSignUp.bind(this);
    this.state = { login: "" };
  }

  render() {
    return (
      <Dialog
        // Specialization
        title="Mars Exploration Program"
        message="How should we refer to you?"
      >
        <input value={this.state.login} onChange={this.handleChange} />
        <button onClick={this.handleSignUp}>Sign Me Up!</button>
      </Dialog>
    );
  }

  handleChange(e) {
    this.setState({ login: e.target.value });
  }

  handleSignUp() {
    alert(`Welcome aboard, ${this.state.login}!`);
  }
}
```

---

## Inheritance 상속

Facebook에서는 수천 개의 React 컴포넌트를 사용하지만, 컴포넌트를 상속 계층 구조로 작성을 권장할만한 사례를 아직 찾지 못했습니다.

props와 합성은 명시적이고 안전한 방법으로 컴포넌트의 모양과 동작을 커스터마이징하는데 필요한 모든 유연성을 제공합니다.

---

## 결론

React는 복잡한 컴포넌트를 쪼개서 여러 개의 컴포넌트로 만든 후 컴포넌트들을 조합해서 새로운 컴포넌트를 만드는 것이 바람직합니다.
