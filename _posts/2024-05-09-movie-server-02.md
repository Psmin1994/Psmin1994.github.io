---
title: Server - 02
author: Psmin
data: 2024-05-09 22:44:37 +0900
categories: [Project, Movie Story]
tags: [Scraping]
---

# DB에 데이터를 추가해보자.

---

## 공공 데이터 활용

첫 번째 방법은 공공 데이터를 제공받을 수 있는 OPEN API를 활용해보는 것이였습니다.

영화진흥위원회의 오픈 API를 활용해보겠습니다.

> 사이트 링크 : [영화진흥위원화](https://www.kobis.or.kr/kobisopenapi/homepg/apiservice/searchServiceInfo.do?serviceId=searchMovieList)

- **첫번째 Get 요청 (일별 박스오피스 API)**

  `날짜(targetDt)`를 parameter로 해당 날짜의 상영 영화들의 대한 정보를 가져옵니다.

  > 사용할 데이터 : 개봉일, 누적관객수

- **두번째 Get 요청 (영화 상세 정보 API)**

  `영화 코드(movieCd)`를 parameter로 영화에 상세 정보 데이터를 가져옵니다.

  > 사용할 데이터 : 영화제목 한/영, 장르, 제작국가, 상영시간, 영화감독, 배우

- **요청 코드**

  ```js
  // 영화진흥위원회 공공 데이터 추출
  import axios from "axios";

  try {
    // 일별 박스오피스 API 요청 주소
    let url =
      "http://www.kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchDailyBoxOfficeList.json";

    let dt = new Date();

    // 가져올 날짜 설정
    let today = `${dt.getFullYear()}${
      dt.getMonth() < 9 ? "0" + (dt.getMonth() + 1) : dt.getMonth() + 1
    }${dt.getDate() - 7}`;

    let res = await axios({
      method: "get",
      url: url,
      params: {
        key: "332ba9ce1cb2f258e6e32ab988458a6c",
        // parameter 로 날짜 지정
        targetDt: today,
      },
    }); // axios(url[, config]) 형태

    // kobis의 일별 박스오피스 Open API로 가져온 설정 날짜의 상영 영화 정보 배열
    let movieList = res.data.boxOfficeResult.dailyBoxOfficeList;

    console.log(movieList[0]);

    // 가져온 데이터에서 영화 상세 정보 데이터를 얻기 위해 영화 코드(movieCd)를 추출
    let movieCd = movieList[0].movieCd;

    // 영화 상세 정보 API 요청 주소
    url = `http://www.kobis.or.kr/kobisopenapi/webservice/rest/movie/searchMovieInfo.json`;

    res = await axios({
      method: "get",
      url: url,
      params: {
        key: "332ba9ce1cb2f258e6e32ab988458a6c",
        // 가져올 영화의 영화 코드 지정
        movieCd,
      },
    }); // axios(url[, config]) 형태

    console.log(res.data.movieInfoResult.movieInfo);
  } catch (err) {
    console.log(err);
  }
  ```

- **응답 데이터**

  ```js
  // 첫번째 응답
  {
    rnum: '1',
    rank: '1',
    rankInten: '0',
    rankOldAndNew: 'OLD',
    movieCd: '20236180', // 영화코드
    movieNm: '웡카',
    openDt: '2024-01-31', // 개봉일 (YYYY-MM-DD)
    salesAmt: '2288686938',
    salesShare: '54.6',
    salesInten: '1358999415',
    salesChange: '146.2',
    salesAcc: '5501692614',
    audiCnt: '226091',
    audiInten: '134065',
    audiChange: '145.7',
    audiAcc: '578105', // 누적관객수
    scrnCnt: '1751',
    showCnt: '8082'
  }

  // 두번째 응답
  {
    movieCd: '20236180',
    movieNm: '웡카',
    movieNmEn: 'Wonka',
    movieNmOg: '',
    showTm: '116',
    prdtYear: '2023',
    openDt: '20240131',
    prdtStatNm: '개봉',
    typeNm: '장편',
    nations: [ { nationNm: '미국' } ],
    genres: [ { genreNm: '판타지' }, { genreNm: '드라마' } ],
    directors: [ { peopleNm: '폴 킹', peopleNmEn: 'Paul King' } ],
    actors: [
      {
        peopleNm: '티모시 샬라메',
        peopleNmEn: 'Timothee Chalamet',
        cast: '',
        castEn: ''
      },
      {
        peopleNm: '칼라 레인',
        peopleNmEn: 'Calah Lane',
        cast: '',
        castEn: ''
      },
      {
        peopleNm: '올리비아 콜맨',
        peopleNmEn: 'Olivia Colman',
        cast: '',
        castEn: ''
      },
      {
        peopleNm: '톰 데이비스',
        peopleNmEn: 'Tom Davis',
        cast: '',
        castEn: ''
      },
      {
        peopleNm: '휴 그랜트',
        peopleNmEn: 'Hugh Grant',
        cast: '',
        castEn: ''
      },
      {
        peopleNm: '샐리 호킨스',
        peopleNmEn: 'Sally Hawkins',
        cast: '',
        castEn: ''
      }
    ],

    ...

  }
  ```

  - **개봉일** : `openDt (String)`
  - **영화제목 한/영** : `movieNm (String)` / `movieNmEn (String)`
  - **장르** : `genres (Array)`
  - **제작국가** : `nations (Array)`
  - **상영시간** : `showTm (String)`
  - **영화감독** : `directors (Array)`
  - **배우** : `actors (Array)`

---

### 문제점

1. 현재 날짜에 대한 데이터를 제공하지않는다.

2. 줄거리, 포스터사진 등 원하는 데이터를 따로 구해야한다.

3. OPEN API를 사용하기 위해 필요한 Key를 주기적으로 갱신해주어야한다.

이러한 단점은 현재 개봉 중인 영화를 실시간으로 제공하려는 서비스에는 맞지 않다.

---

## 웹 스크래핑 (Scraping)

> 참고 글 : [puppeteer](https://psmin1994.github.io/posts/puppeteer/)

두 번째 방법으로 puppeteer를 사용한 웹 스크래핑을 활용해보겠습니다.

puppeteer란 **Node.js용 Chrome/Chrominm 제어 API**로 DevTools 프로토콜을 이용하여 브라우저를 제어할 수 있습니다.

[네이버](https://m.naver.com/)에서 현재 상영 영화에 대한 데이터를 추출해보겠습니다.

---

### getList

네이버 홈페이지에서 **현재 상영 영화**와**개봉 예정 영화**를 검색하여 나온 영화 목록에서 **a 태그의 href 속성**을 추출해 배열로 반환합니다.

```js
import puppeteer from "puppeteer";

var getList = async () => {
  try {
    let result = [];

    function sleep(ms) {
      var start = Date.now() + ms;
      while (Date.now() < start) {}
    }

    const crawlList = async () => {
      await page.waitForSelector(
        "div.cm_content_wrap > div > div > div > div.card_content._result_area > div.card_area._panel > div:last-child"
      );

      let movieList = await page.$$(
        "div.cm_content_wrap > div > div > div > div.card_content._result_area > div.card_area._panel > div"
      );

      for (let movie of movieList) {
        let href = await movie.$eval(
          "div.data_area > div > div.title > div > a",
          (el) => {
            return el.href;
          }
        );

        result.push(
          href[0] == "?" ? "https://search.naver.com/search.naver" + href : href
        );
      }
    };

    // 옵션으로 headless모드를 끌 수 있다.
    const browser = await puppeteer.launch({
      headless: true,
    });

    const page = await browser.newPage();

    await page.goto("https://www.naver.com/");

    await page.type("#query", `현재 상영 영화`);

    await page.click("#sform > fieldset > button");

    await page.waitForSelector(
      "div.card_content._result_area > div.cm_paging_area._page > div > span > span._total"
    );

    let pageNum = await page.$eval(
      "div.card_content._result_area > div.cm_paging_area._page > div > span > span._total",
      (el) => {
        return el.textContent;
      }
    );

    for (let i = 1; i < pageNum; i++) {
      await crawlList();

      await page.click(
        "div.card_content._result_area > div.cm_paging_area._page > div > a.pg_next.on._next"
      );

      sleep(1000);
    }

    await crawlList();

    return [result, browser, page];
  } catch (err) {
    throw err;
  }
};

export default getList;
```

---

### insertData

`getList.js`에서 반환한 href 배열을 하나씩 이동하며 **영화 정보**, **출연진 정보**, **사진** 등을 추출하여 DB에 삽입합니다.

```js
import getImgUrl from "./getImgUrl.js";
import movieStorage from "./model/movieInsert.model.js";

var insertData = async (urlList, page) => {
  try {
    let cnt = 1;
    let total = urlList.length;

    for (let href of urlList) {
      let index = 2;

      let movieData = {};
      let genreData = [];

      console.log(`${cnt++} / ${total} 진행 중 ...`);

      // 영화 기본 정보 페이지로 이동
      await page.goto(href);

      await page.waitForSelector(
        "div.sub_tap_area > div > div > ul > li:last-child > a"
      );

      let navArr = [];

      let navList = await page.$$("div.sub_tap_area > div > div > ul > li");

      for (let node of navList) {
        navArr.push(await node.$eval("a > span.menu", (el) => el.textContent));
      }

      // 정보 페이지 이동
      await page.waitForSelector(
        `div.sub_tap_area > div > div > ul > li:nth-child(${index}) > a`
      );

      await page.click(
        `div.sub_tap_area > div > div > ul > li:nth-child(${index}) > a`
      );

      await page.waitForSelector("div.detail_info > dl > div:last-child");

      // 영화 데이터 추출
      movieData.movie_nm = await page.$eval(
        "div.cm_top_wrap > div.title_area > h2 > span.area_text_title > strong",
        (el) => {
          return el.textContent;
        }
      );

      let isExist = await movieStorage.existMovieNm(movieData.movie_nm);

      if (isExist) continue;

      movieData.movie_nm_en = await page.$eval(
        "div.cm_top_wrap > div.title_area > div > span:nth-child(3)",
        (el) => {
          return el.textContent;
        }
      );

      let infoList = await page.$$("div.detail_info > dl > div");

      for (let node of infoList) {
        const key = await node.$eval("dt", (element) => {
          return element.textContent;
        });

        const value = await node.$eval("dd", (element) => {
          return element.textContent;
        });

        switch (key) {
          case "개봉":
            movieData.open_date = value.slice(0, -1).replaceAll(".", "-");
            break;

          // 장르 데이터는 따로 저장
          case "장르":
            let tmp = value.split(", " || "/");

            for (let genre of tmp) genreData.push(genre);
            break;
          case "국가":
            movieData.nation = value;
            break;
          case "러닝타임":
            movieData.showtime = value.replace("분", "") * 1;
            break;
          default:
            break;
        }
      }

      await page.waitForSelector(
        "div.cm_content_wrap > div.cm_content_area > div > div.intro_box._content > p"
      );

      movieData.summary = await page.$eval(
        "div.cm_content_wrap > div.cm_content_area > div > div.intro_box._content > p",
        (el) => {
          return el.textContent;
        }
      );

      movieData.poster = "";
      movieData.stillCut = "";

      // 포토 페이지가 있을 경우
      if (navArr.includes("포토")) {
        index = navArr.indexOf("포토") + 1;

        let stillCutArr = [];

        await page.waitForSelector(
          `div.sub_tap_area > div > div > ul > li:nth-child(${index}) > a`
        );

        // 포토 페이지로 이동
        await page.click(
          `div.sub_tap_area > div > div > ul > li:nth-child(${index}) > a`
        );

        await page.waitForSelector(
          "div.cm_content_wrap > div > div > div > div:last-child > div > div.movie_photo_list._list > div > ul > li:nth-child(1) > a > img"
        );

        let photoList = await page.$$(
          "div.cm_content_wrap > div > div > div > div.area_card"
        );

        for (let node of photoList) {
          let checked = await node.$eval("div > h3 > strong", (el) => {
            return el.textContent;
          });

          if (checked == "포스터") {
            let posterUrl = await node.$eval(
              "div > div.movie_photo_list._list > div > ul > li:nth-child(1) > a > img",
              (el) => {
                return el.src;
              }
            );

            movieData.poster = await getImgUrl(
              posterUrl,
              movieData.movie_nm_en,
              `movie/${movieData.movie_nm_en}/poster`
            );
          } else if (checked == "스틸컷") {
            let cutList = await node.$$(
              "div > div.movie_photo_list._list > div > ul > li"
            );

            // 영화 스틸 컷 6장 까지만 추출
            if (cutList.length > 6) cutList.length = 6;

            for (let i = 0; i < cutList.length; i++) {
              let imgUrl = await cutList[i].$eval("a > img", (el) => {
                return el.src;
              });

              let newImgUrl = await getImgUrl(
                imgUrl,
                `${movieData.movie_nm_en}_${i}`,
                `movie/${movieData.movie_nm_en}/stillcut`
              );

              stillCutArr.push(newImgUrl);
            }

            movieData.stillCut = JSON.stringify(stillCutArr);
          }
        }
      }

      // *************************************************
      // movie 테이블 삽입
      let movieId = await movieStorage.insertMovie(movieData);

      // genre 테이블, 연결 테이블 삽입
      for (let genreNm of genreData) {
        let isExist = await movieStorage.existGenreNm(genreNm);

        let genreId = isExist
          ? await movieStorage.getGenreId(genreNm)
          : await movieStorage.insertGenre(genreNm);

        await movieStorage.insertMovieAndGenre(movieId, genreId);
      }

      // *****************************************************
      // 감독/출연 페이지로 이동
      index = navArr.indexOf("감독/출연") + 1;

      await page.waitForSelector(
        `div.sub_tap_area > div > div > ul > li:nth-child(${index}) > a`
      );

      await page.click(
        `div.sub_tap_area > div > div > ul > li:nth-child(${index}) > a`
      );

      await page.waitForSelector("div.cast_box");

      let castList = await page.$$("div.cast_box");

      if (castList.length > 3) castList.length = 3;

      for (let node of castList) {
        let isActor = await node.$eval("h3", (el) => {
          return el.textContent == "감독" ? false : true;
        });

        let itemList = await node.$$("ul > li");

        for (let item of itemList) {
          let castNm = await item.$eval("span", (el) => {
            return el.textContent;
          });

          if (castNm.trim() == "더보기") continue;

          let [imgUrl, imgName] = await item.$eval("div.thumb > img", (el) => {
            return [el.src, el.alt];
          });

          let role = isActor ? "actor" : "director";

          imgUrl =
            imgName == "이미지 준비중"
              ? "c:\\workspace\\project\\Movie-Story\\server\\src\\public\\img\\movie\\이미지 준비중.png"
              : await getImgUrl(imgUrl, imgName, `${role}`);

          // ************************************************
          // actor, director 테이블 삽입
          let isExist = await movieStorage.existCastNm(castNm, role);

          let castId = isExist
            ? await movieStorage.getCastId(castNm, role)
            : await movieStorage.insertCast(imgUrl, castNm, role);

          await movieStorage.insertMovieAndCast(movieId, castId, role);
        }
      }
    }

    // 브라우저를 종료한다.
    await browser.close();
  } catch (err) {
    throw err;
  }
};

export default insertData;
```

---

### getImgUrl

DB에 이미지를 저장할 때 이미지 자체가 아닌 **이미지 파일의 경로를 문자열**로 저장합니다.

이미지 파일을 **public 폴더에 저장**한 후 **해당 경로를 문자열로 반환**합니다.

```js
import path from "path";
import fs from "fs";
import axios from "axios";

let dirname = import.meta.dirname;

var getImgUrl = async (imgUrl, imgName, imgPath) => {
  try {
    // 저장될 이미지 경로
    let newPath =
      `${dirname}/../src/public/img/` + `${imgPath}`.replaceAll(":", "-");

    // 생성될 이미지 파일명
    let newName = imgName.replaceAll(":", "-").replaceAll("/", "_");

    // 경로 중 존재하지 않는 폴더 생성
    if (!fs.existsSync(newPath)) fs.mkdirSync(newPath, { recursive: true });

    // 이미지 가져오기
    const imgResult = await axios.get(imgUrl, {
      responseType: "arraybuffer",
    });

    // 확장자 추출
    let extension = imgUrl.slice(imgUrl.lastIndexOf(".") - imgUrl.length);

    // 생성될 이미지 최종 경로
    const newImgUrl = path.normalize(`${newPath}/${newName}${extension}`);

    // 이미지 생성
    await fs.writeFileSync(newImgUrl, imgResult.data);

    // 이미지 경로 반환
    return newImgUrl;
  } catch (err) {
    throw err;
  }
};

export default getImgUrl;
```

---

### index.js

위 코드들을 실행하는 index 코드로 매일 오전 6시에 스케줄링 해둘 예정입니다.

```js
import getList from "./getList.js";
import insertData from "./insertData.js";

let scraping = async () => {
  try {
    let [urlList, browser, page] = await getList();

    await insertData(urlList, page);
  } catch (err) {
    throw err;
  }
};

export default scraping;
```
