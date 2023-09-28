---
title: sanitize-html 모듈
author: Psmin
data: 2023-02-28 14:53:41 +0900
categories: [Knowledge, Nodejs]
tags: [sanitize-html]
---

## sanitize-html 모듈

HTML의 **input** 또는 **textarea** 등의 사용자 입력정보에 `<script>문자</script>`를 사용해 웹 브라우저에서 **txt가 아닌 script로 받아들이는 악성 스크립트를 필터링**하기 위한 node의 패키지 모듈입니다.

즉, `<script>`, `<a>` 등등 기타 태그들을 치환시켜 악성 스크립트를 막아주는 보안 라이브러리입니다.

만약, 게시판이라는 웹 서비스를 운영하고 있는데, 누군가 게시글 본문 내용에 `<script>location.href='악성 웹사이트 url'<script>`를 작성했다고 하자.

서버 측에서 조치를 취하지 않는다면 **게시물에 접근했을 때 악성 웹 사이트로 리다이렉트**되어 공격 받을 수 있습니다.

- **설치**

  ```
  npm i sanitize-html
  ```

---

## 사용방법

대부분의 태그 문자열을 비허용하여 필터링하지만, **사용자 지정으로 특정한 태그를 지정해 허용**시킬 수 있습니다.

만약, `allowedTags: false, allowedAttributes: false` 와 같은 옵션을 주면, 모든 태그와 속성을 허용합니다.

반대로 `allowedTags: [], allowedAttributes: []` 와 같은 옵션은 모든 태그를 금지합니다.

```js
import sanitizeHtml from "sanitize-html";

const dirty = `h1태그는 <h1>링크</h1> 무시가 될까?`;

const sanitizedDescription = sanitizeHtml(dirty, {
  allowedTags: ["h1", "a"], // h1 , a 태그 허용
  allowedAttributes: { a: ["href"] }, // a 태그의 href 속성 허용
  allowedFrameHostnames: ["www.youtube.com"], // iframe 허용하되 유튜브 사이트만 허용
});

console.log(sanitizedDescription); // 출력 : h1태그는 <h1>링크</h1> 무시가 될까?
```

---

### 허용 범위 기본 값

추가 옵션을 지정하지않고 그대로 `sanitizeHTML(dirty)` 로 쓴다면, **2.11.0 버전 기준** 허용되는 태그와 허용되지 않는 태그 및 기타 default 설정은 다음과 같습니다.

```js
sanitizeHtml(dirty, {
    allowedTags: [
  "address", "article", "aside", "footer", "header", "h1", "h2", "h3", "h4",
  "h5", "h6", "hgroup", "main", "nav", "section", "blockquote", "dd", "div",
  "dl", "dt", "figcaption", "figure", "hr", "li", "main", "ol", "p", "pre",
  "ul", "a", "abbr", "b", "bdi", "bdo", "br", "cite", "code", "data", "dfn",
  "em", "i", "kbd", "mark", "q", "rb", "rp", "rt", "rtc", "ruby", "s", "samp",
  "small", "span", "strong", "sub", "sup", "time", "u", "var", "wbr", "caption",
  "col", "colgroup", "table", "tbody", "td", "tfoot", "th", "thead", "tr"
],
nonBooleanAttributes: [
  'abbr', 'accept', 'accept-charset', 'accesskey', 'action',
  'allow', 'alt', 'as', 'autocapitalize',

  ...


],
disallowedTagsMode: 'discard',
allowedAttributes: {
  a: [ 'href', 'name', 'target' ],
  // We don't currently allow img itself by default, but
  // these attributes would make sense if we did.
  img: [ 'src', 'srcset', 'alt', 'title', 'width', 'height', 'loading' ]
},
// Lots of these won't come up by default because we don't allow them
selfClosing: [ 'img', 'br', 'hr', 'area', 'base', 'basefont', 'input', 'link', 'meta' ],
// URL schemes we permit
allowedSchemes: [ 'http', 'https', 'ftp', 'mailto', 'tel' ],
allowedSchemesByTag: {},
allowedSchemesAppliedToAttributes: [ 'href', 'src', 'cite' ],
allowProtocolRelative: true,
enforceHtmlBoundary: false,
parseStyleAttributes: true
}
```

---

## 미들웨어 만들기

sanitize-html을 직접 사용자 미들웨어로 만들어보겠습니다.

게시판에서 게시글을 작성하는 POST 요청이 왔을때 **content 문자열에 문제되는 태그들을 필터링**하고 다음 미들웨어로 넘기는 형태로 구성해보겠습니다.

```js
// sanitizer.js
const sanitizeHtml = require("sanitize-html");

const sanitizeOption = {
  allowedTags: ["h1", "h2", "p", "ul", "ol", "li", "a", "img"],
  allowedAttributes: {
    a: ["href", "name", "target"],
    img: ["src"],
    li: ["class"],
  },
  allowedSchemes: ["data", "http"],
};

exports.sanitizer = (req, res, next) => {
  const filtered = sanitizeHtml(req.body.content, sanitizeOption); // 게시글 내용 req.body.content를 sanitize하여 결과 문자열을 변수에 저장
  filtered.length < 200 ? filtered : `${filtered.slice(0, 200)}...`; // 게시글 내용은 200자 제한이 있다면
  req.filtered = filtered; // 새로만든 req.filtered 객체에 소독한 문자열을 저장
  next(); // 다음 미들웨어로
};
```

```js
const { sanitizer } = require("./sanitizer");

// ...

// article/post 로 POST 요청이 오면, 사용자 미들웨어 sanitezer에서 req.content 문자열 값을 소독하고 다음 미들웨어로 넘긴다
app.post("/article/post", sanitizer, async (req, res, next) => {
  const post = await Post.create({
    title: req.body.title, // 게시글 제목
    content: req.filtered, // sanitizer 사용자 미들웨어에서 req.content를 필터링해서 만든 게시글 내용 객체
    UserId: req.user.id, // 게시글 작성자
  });
});
```
