---
title: nodemon.json
author: Psmin
data: 2024-10-04 15:23:17 +0900
categories: [Knowledge, Typescript]
tags: [tsconfig]
---

## nodemon.json

Nodemon을 사용할 때 설정을 관리하기 위한 JSON 파일입니다.

> Nodemon이란?  
> Node.js 애플리케이션 개발 중에 파일 변경을 감지하고 자동으로 애플리케이션을 다시 시작해주는 도구

- **기본 구조**

  ```json
  {
    "watch": ["src"],
    "ext": "js,json",
    "ignore": ["node_modules"],
    "exec": "node app.js"
  }
  ```

---

---

## 주요 설정

- **watch** : 감시할 파일이나 디렉토리

- **ext** : 감시할 파일의 확장자

- **ignore** : 감시에서 제외할 파일이나 디렉토리

- **exec** : 파일 변경 시 실행할 명령어

- **delay** : 파일 변경 후 실행 지연 시간

- **restartable** : 실행 중 재시작 명령어

- **env** : 실행 시 사용할 환경 변수

- **verbose** : 상세 로그 출력을 활성화

```json
{
  "watch": ["src"],
  "ext": "ts,js,json",
  "exec": "ts-node src/index.ts",
  "ignore": ["node_modules"]
}
```
