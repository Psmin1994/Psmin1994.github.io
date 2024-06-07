---
title: puppeteer 모듈
author: Psmin
data: 2024-02-06 16:22:32 +0900
categories: [Knowledge, Nodejs]
tags: [puppeteer, Scraping]
---

## puppeteer 모듈에 대해 알아보자.

---

## puppeteer 모듈

구글에서 개발한 **Node.js용 Chrome/Chrominm 제어 API**로 DevTools 프로토콜을 이용하여 브라우저를 제어합니다.

마우스, 키보드, 쿠키 및 세션 스토리지 등 여러 자원들을 제어할 수 있습니다.

주로 웹 테스트, 웹 스크래핑, 웹 크롤링 등 **웹 자동화 작업을 수행**할 때 사용합니다.

- **설치**

  ```console
  npm i puppeteer
  ```

---

## puppeteer 구조

puppeteer API는 계층적인 구조로 구성되어 있습니다.

- Browser 인스턴스는 여러 browser context를 갖는다.
- BrowserContext 인스턴스는 실제 사용할 떄의 새창을 의미하며 여러 Page를 갖습니다.
- Page는 하나의 창에 새 탭을 의미하며 여러 Frame을 갖습니다.

![puppeteer](/assets/img/puppeteer.png){: .normal}

---

## puppeteer API

- `puppeteer.launch(option)` : Browser 인스턴스를 생성합니다.

  - **option**

    - headless (default : true) : false로 설정 시, 브라우저를 실행
    - defaultViewport (default " {width: 800, height: 600}) : 화면 사이즈 지정
    - devtools (default : false) : true로 설정 시, 브라우저의 Devtools가 열립니다.

---

- `browser.createBrowserContext()` : Browser 인스턴스에 새로운 Browser Context를 생성한다.  
  (새 창을 생성합니다.)

- `browser.close()` : Browser 종료

---

- `browserContext.newPage()` : 생성된 Browser Context에 새로운 Page를 생성합니다.  
  (열린 창에 새로운 탭을 생성합니다.)

  > 첫 페이지를 생성하지 않으면 창이 열리지 않음.

---

- `page.goto(url)` : 페이지가 url 주소로 이동합니다.

- `page.click(selector)` : 페이지에서 CSS selector로 지정된 Element를 클릭합니다.

- `page.type(selector, text, option)` : 페이지에서 CSS selector로 지정된 Element에 text문자를 입력합니다.

  - **option**

    - delay (default : 0) : 입력 시 딜레이를 지정

- `page.waitForSelector(selector, options)` : 페이지에 해당 CSS selector 요소가 나타날 때까지 기다립니다.

  - **option**

    - timeout (default : 30_000) : 나타나길 기다릴 최대 시간을 지정합니다. (밀리초 단위)

---

- `page.$(selector)` : document.querySelector를 실행합니다.

  즉, CSS selector로 지정된 Element를 하나 가져옵니다. (없을 시, null)

- `page.$$(selector)` : document.querySelectorAll를 실행합니다.

  즉, CSS selector로 지정된 모든 Element를 배열로 가져옵니다. (없을 시, [])

- `page.$eval(selector, pageFunction, args)` : CSS selector로 지정된 Element를 pageFunction에 인수로 넘겨주고 함수를 실행합니다.

- `page.$$eval(selector, pageFunction, args)` : CSS selector로 지정된 모든 Element들의 배열을 pageFunction에 인수로 넘겨주고 함수를 실행합니다.

---

## 데이터 추출

`$()`, `$$()`는 **ElementHandle**를 반환하기 때문에 바로 데이터를 추출할 수 없고 `evaluate()`를 통해 데이터를 추출해야합니다.

`$eval()`, `$$eval()`는 `evaluate()`를 내장하고 있기 때문에 **Element**를 바로 Callback 함수에 전달해주어 비로 데이터를 추출할 수 있습니다.

```js
// $eval
let title1 = await page.$eval(".title", (el) => {
  return el.textContent;
});

// $ + evaluate
let elHandle = await page.$(".title");
let title2 = elHandle.evaluate((el) => {
  return el.textContent;
});

console.log(title1 == title2); // true
```

> pageFunction 에서 사용하는 Node 프로퍼티 참고 : [Node - Mdn](https://developer.mozilla.org/en-US/docs/Web/API/Node)
