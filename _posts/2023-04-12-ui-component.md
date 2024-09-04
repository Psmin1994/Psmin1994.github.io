---
title: UI Component
author: Psmin
data: 2023-04-12 14:13:52 +0900
categories: [Knowledge, ReactJS]
tags: [React, UI]
---

## UI Component

**사용자와 상호작용**하는 기본적인 형태로 어느 곳에서도 **자신의 기능을 수행하는 가장 작은 단위**를 말합니다.

좋은 컴포넌트는 <u>재사용이 가능하고, 일관성과 통일성이 있어 사용자들이 항상 동일한 기능으로 인식</u>할 수 있어야합니다.

---

## Component의 종류

사용자가 조작할 때를 기준으로 3가지로 나뉘어집니다.

![compo-categories](/assets/img/compo-categories.png){: }

- **탐색 Navication**

  <u>정보를 탐색할 때</u> 사용하는 컴포넌트입니다.

  주로 정보를 담거나 압축된 정보를 펼치는 등 정보를 보여줍니다.

- **입력 Input**

  <u>정보를 입력할 때</u> 사용하는 컴포넌트입니다.

  사용자의 선택이나 구체적인 정보를 받으며, 받는 정보에 따라 여러 컴포넌트들을 사용합니다.

- **정보 Information**

  <u>정보를 전달할 때</u> 사용하는 컴포넌트입니다.

  사용자가 선택해야하는 정보 등 상황에 맞게 정보를 제공할 때 사용합니다.

---

## Component 디자인에서 고려할 점

- **형태 Shape**

  형태를 통해 다른 요소와 구분하며 전체 서비스에서 일관되게 사용해서 **사용자가 동작을 예측**할 수 있도록합니다.

  모양, 색상, 크기, 아이콘, 텍스트 등으로 용도에 맞게 주목도를 조절합니다.

  ![compo-shape](/assets/img/compo-shape.png){: }

- **상태 Status**

  터치나 클릭에 대한 반응 등 **현재 컴포넌트의 상태를 알려주어 사용자가 상황을 파악**할 수 있도록 도와줍니다.

  현재 사용할 수 없을 때 클릭할 수 없게하거나, 로딩중을 표시해 기다려야하는 상황을 알려주기도 합니다.

  ![compo-status](/assets/img/compo-status.png){: }

- **맥락 Context**

  같은 컴포넌트라도 **여러 환경에서 여러 쓰임새에 따라 다양한 형태로 표현**합니다.

  ![compo-context](/assets/img/compo-context.png){: }
