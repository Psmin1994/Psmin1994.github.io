---
title: Node.js의 body-parser 모듈
author: Psmin
data: 2022-12-27 16:23:53 +0900
categories: [Nodejs]
tags: [Body-Parser]
---

# body-parser에 대해 알아보자.

---

## body-parser

HTTP 의 post, put 요청시 HTTP 의 본문(body)를 parsing 하여 나온 결과값을 req.body 에 넣어 body 프로퍼티를 사용할 수 있도록 합니다.

아래의 테스트 코드에서 req.body를 콘솔로그로 출력해보면 undefined가 출력합니다.

즉, Node.js의 웹프레임워크인 Express는 요청을 처리할 때 기본적으로 body를 undefined로 처리하고 있습니다.

```js
app.get("/test", (req, res) => {
  res.send("test");
  console.log(req.body);
});
```

body-parser 모듈로 미들웨어 함수를 등록하면, request의 body부분을 자신이 원하는 형태로 파싱하여 활용할 수 있습니다.

---

## body-parser의 parser

1. JSON body parser
2. URL-encoded from body parser
3. Raw body parser
4. Text body Parser

위 4가지 parser 중에서 가장 많이 사용되는 **_JSON body parser_** 와 **_URL-encoded from body parser_** 에 대해 알아보겠습니다.

---

### JSON body parser

HTTP 의 본문(body)를 **_JSON_** 으로 파싱합니다.
HTTP 의 헤더(header) **_Content-Type_** 속성 값이 **_“application/json”_** 이 아닐 경우에는 파싱하지 않습니다.

- 옵션

  - `inflate` (default : <kbd>true</kbd>)  
    압축된 요청 body의 처리를 허용합 지 설정합니다.

  - `limit` (default : <kbd>100kB</kbd>)  
    최대 요청 body의 크기를 제한합니다.

  - `reviver` (default : <kbd>null</kbd>)  
    **_JSON.parse()_** 의 두 번째 인자로 직접 전달됩니다.  
    reviver가 주어지면 분석한 값을 반환하기 전에 변환합니다.

    ```js
    JSON.parse('{"p": 5}', (key, value) =>
      typeof value === "number" ? value * 2 : value
    );
    // { p: 10 }
    ```

  - `strict` (default : <kbd>true</kbd>)  
    true로 설정 시 배열과 객체만 파싱  
    false로 설정하면 JSON.parse()가 허용하는 모든 항목 허용  
    (즉, 숫자, 문자열 등의 원시 타입 데이터도 파싱해준다.)

  - `type` (default : <kbd>application/json</kbd>)  
    미들웨어가 파싱할 미디어의 타입을 결정하는 옵션

---

### URL-encoded from body parser

HTTP 의 본문(body)를 **_x-www-form-urlencoded_** 으로 파싱합니다.
HTTP 의 헤더(header) **_Content-Type_** 속성 값이 “x-www-form-urlencoded” 이 아닐 경우에는 파싱하지 않습니다.

- 옵션

  - `extended`  
    false로 설정 시 Node.js에 기본으로 내장된 **_queryString 모듈_**을 사용합니다.  
    true로 설정 시 **_qs 모듈_**을 사용합니다.

    > queryString 모듈 : url 주소 뒤에 붙어서 넘어오는 파라미터인 querystring을 쉽게 조작할 수 있는 기능을 제공하는 모듈

    > qs 모듈 : 별도 설치가 필요한 queryString 모듈의 기능보다 확장된 모듈

---

## body-parser 설정

**_application/json_** 은 `{key: value}` 형태이고, **_application/x-www-form-urlencoded_**는 `key=value&key=value`의 형태입니다.

즉, JSON 형식으로 { name: 'psmin', book: 'nodejs' } 를 본문으로 보낸다면 req.body에 그대로 들어가며 URL-encoded 형식으로 name=psmin&book=nodejs를 본문으로 보낸다면
req.body에 { name: 'psmin', book: 'nodejs' } 가 들어갑니다.

Express 4.16.0 버전 이상부터 내장모듈로 포함됐기 때문에 별도로 설치할 필요 없이 간단한 설정을 통해 바로 사용할 수 있습니다.

```js
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
```

{: prompt-info}

> 서버 코드에 적용해볼까요?

---

## 서버 코드

```js
import express from "express";
import bodyParser from "body-parser";
import path from "path";

const app = express();
const __dirname = path.resolve();

app.use("/", express.static(`${__dirname}/src/public`));

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));

// route
app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.get("/test", (req, res) => {
  console.log(req.body);
  var msg = `name : ${req.body.name}\nage : ${req.body.age}`;
  res.send(msg);
});

app.post("/test", (req, res) => {
  console.log(req.body);
  var msg = `name : ${req.body.name}\nage : ${req.body.age}`;
  res.send(msg);
});

// 3000 포트로 서버 오픈
app.listen(3000, () => {
  console.log("express server on port 3000");
});
```

---

## Postman으로 테스트 해보기

Postman를 이용하여 HTTP 본문(body) 에 name 과 age 프로퍼티를 갖는 JSON 객체를 보내보겠습니다.

여기서 주의할 점은 HTTP 본문(body) 의 type 이 JSON 이라는 것을 설정해줘야 합니다.

![Body-parser-01](/assets/img/postman-bodyparser-01.png){: .w-80 .normal}
req.body.name 과 req.body.age가 잘 확인됩니다.

다음은 HTTP 본문(body) 에는 name 과 age key를 갖는 form 데이터를 넣어보겠습니다.

여기서도 주의할 점은 보내고자하는 HTTP 본문(body) 의 type 이 “x-www-form-urlencoded” 이라는 것을 설정해줘야 합니다.

![Body-parser-02](/assets/img/postman-bodyparser-02.png){: .w-80 .normal}
잘 접근되는 것을 확인 할 수 있습니다.

---

## 참조

- <https://velog.io/@wlwl99/body-parser-%EB%AF%B8%EB%93%A4%EC%9B%A8%EC%96%B4>
- <https://dev1-5414.medium.com/body-parser-5d4429b4b6bc>
- <https://backback.tistory.com/336>
- <https://cheony-y.tistory.com/267>
