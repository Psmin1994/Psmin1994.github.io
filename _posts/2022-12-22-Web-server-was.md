---
title: Web Server와 Was
author: Psmin
data: 2022-12-22 02:39:23 +0900
categories: [Knowledge]
tags: [Web Server, Was]
---

# Web Server와 Was에 대해 알아보자.

---

## Web Server란?

![Web-Server](/assets/img/web-server.jpg){: .w-80 .normal}

브라우저와 같은 Client로부터 HTTP 프로토콜 요청을 받으면 HTML 문서 등의 정적 웹 페이지 를 응답해주는 소프트웨어를 말합니다.

대표적으로 Apache, NginX, WebtoB 등이 있으며, 동적 컨텐츠를 요청 받으면 WAS에게 요청을 넘겨주고, WAS에서 처리한 결과를 Client에 전달하는 역할도 합니다.

- 주요 기능
  - 정적 컨텐츠 제공
  - 동적 컨텐츠 제공을 위한 요청 전달

---

## WAS (Web Application Server)

DB 조회나 다양한 로직 처리를 요두하는 동적인 컨텐츠를 제공하기 위해 만들어진 Application Server를 말합니다.

HTTP 프로토콜을 기반으로 수행되는 미들웨어로 주로 DB 서버와 같이 수행합니다.  
또한, JSP, Servlet 구동 환경을 제공하기에 “웹 컨테이너(Web Container)” 혹은 “서블릿 컨테이너(Servlet Container)”라고도 부릅니다.

WAS는 Web Server의 기능들을 구조적으로 분리하여 처리하기위해 제시되었습니다.  
크게 Web Server의 기능과 컨테이너의 기능으로 구성됩니다.

대표적으로 Tomcat, JBoss, WebSphere 등이 있습니다.

- 주요 기능
  - 프로그램 실행 환경 제공
  - DB 접속 기능 제공
  - 여러 트랜잭션 관리
  - 비즈니스 로직 수행

---

## Web Server VS WAS

Web Server는 정적 컨텐츠를 처리하고 WAS는 동적 컨텐츠를 처리합니다.

하지만 요즘의 WAS는 Web Server 기능을 내장하고 있어 정적 컨텐츠 처리하는 성능상 큰 차이가 없다고합니다.

{: .prompt-info}

> 그렇다면 Web Server는 왜 필요할까요?

- **_Web Server가 필요한 이유_**

  Client는 서버로부터 HTML 문서를 먼저 받은 후 필요에 따라 이미지 파일과 같은 정적 파일들을 요청합니다.  
  Web Server는 요청받은 정적 파일들을 Application Server까지 가지 않고 먼저 빠르게 보내줄 수 있습니다.  
  즉, Web Server에서 빠르게 정적 컨텐츠만 처리하도록 기능의 분배를 통해 서버의 부하를 줄일 수 있습니다.

- **_WAS와 Web Server 분리하는 이유_**

  - 큰 규모의 프로젝트에서의 자원 이용의 <kbd>효율성 및 장애 극복</kbd>
  - <kbd>배포 및 유지보수의 편의성</kbd>
    (예시 : Apache + Tomcat)

- **_결론_**

  ![Web-Server](/assets/img/web-server.jpg){: .w-80 .normal}

  Web Server를 WAS 앞에 두고 처리해 WAS를 Web Server가 필요에 따라 요청할 수 있는 플러그인 형태로 설정하면 효율적인 분산 처리가 가능할 것입니다.

---

## 참조

- <https://code-lab1.tistory.com/199>
- <https://aonenetworks.tistory.com/616>
- <https://velog.io/@zzz0000227/%EC%9B%B9%EC%84%9C%EB%B2%84%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80#wasweb-application-server>
- <https://byul91oh.tistory.com/80>
