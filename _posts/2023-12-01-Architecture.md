---
title: Nodejs 프로젝트 설계하기
author: Psmin
data: 2023-05-22 16:29:12 +0900
categories: [Knowledge, Nodejs]
tags: [architecture]
---

## 어떻게 Nodejs 프로젝트를 견고하게 설계할 지 고민해보자.

---

### 고민해보는 이유

Express.js는 node.js REST API를 만드는데 좋은 프레임워크이지만 구조를 알려주지는 않습니다.

올바른 프로젝트 구조는 코드의 중복을 피해주며 유지보수하기 편하게하며 협업과 서비스 확장에 유리합니다.

이번 포스팅의 목적은 잘 짜여진 아키텍쳐 구조를 따라가보며 좋은 설계가 무엇인지 생각해보겠습니다.

---

##
