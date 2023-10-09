---
title: Shared State
author: Psmin
data: 2023-04-02 11:24:47 +0900
categories: [Knowledge, ReactJS]
tags: [React, State Share]
---

## Shared State

어떤 컴포넌트의 State에 있는 데이터를 **여러 개의 하위 컴포넌트에서 공통적으로 사용**하는 경우를 말합니다.

![Shared-State](/assets/img/shared-state.png){: .w-80}

위 그림의 자식 컴포넌트 A, B는 부모 컴포넌트의 value에 각각 2, 3을 곱해서 표시하는 컴포넌트입니다.

이러한 경우 자식 컴포넌트들은 값을 갖을 필요없이 부모 컴포넌트의 state에 있는 값에 각각 2, 3을 곱한 후 표시하면 됩니다.

> 즉, 동일한 데이터에 대한 변경사항을 여러 컴포넌트에 반영해야 할 필요가 있을 때 가장 가까운 공통 상위 컴포넌트로 state를 끌어올리는 것이 좋습니다. (Lifting State Up)

---

## 하위 컴포넌트에서 state 공유해보기

- **상위 컴포넌트**

  ```js
  import { useState } from "react";
  import ChildInput from "./ChildInput";

  function ShareValue() {
    // 상위 컴포넌트의 state에 'First Value' 라는 초기값을 갖는 state를 정의합니다.
    const [value, setValue] = useState("First Value");

    // value를 변경하는 handle 함수
    const handleChangeValue = (childStateValue) => {
      setValue(childStateValue);
    };

    return (
      // 하위 컴포넌트에서 상위 컴포넌트의 state와 함수를 props로 받아옵니다.
      <div>
        <ChildInput value={value} onChangeValue={handleChangeValue} />
        <p>ChildInput 의 value: {value}</p>
      </div>
    );
  }
  ```

- **하위 컴포넌트**

  ```js
  function ChildInput(props) {
    const handleChange = (event) => {
      // 하위 컴포넌트 내부에서 props로 받아온 함수를 사용하여 input의 value를 넘기고 상위 컴포넌트의 value에 대입합니다.
      props.onChangeValue(event.target.value);
    };
    return (
      // 하위 컴포넌트 내부에서 props로 받아온 state를 사용합니다.
      <div>
        <input type="text" value={props.value} onChange={handleChange} />
      </div>
    );
  }
  ```
