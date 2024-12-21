---
title: Server - 09
author: Psmin
data: 2024-02-27 03:23:21 +0900
categories: [Project, Movie Story]
tags: [passport, oauth]
---

# OAuth 2.0 인증을 구현해보자.

Naver 계정을 통해 OAuth 인증을 구현해보겠습니다.

> 참고글 : [OAuth](https://psmin1994.github.io/posts/oauth/)

---

## OAuth 서비스 등록

우선, OAuth 2.0 로그인을 사용하기 위해서는 이용하고자 하는 Resource Server에 자신의 서비스(어플리케이션)을 등록해야합니다.

[Naver Developers 페이지](https://developers.naver.com/main/)로 이동합니다.

'Application - 어플리케이션 등록' 으로 이동합니다.

![naver-oauth-01](/assets/img/naver-oauth-01.png)

어플리케이션 이름을 작성하고 사용 API에서 **네이버 로그인**을 선택합니다.
