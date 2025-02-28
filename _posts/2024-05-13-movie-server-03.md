---
title: Server - 03
author: Psmin
data: 2024-05-13 23:15:33 +0900
categories: [Project, Movie Story]
tags: [express, MVC]
---

# express 모듈 기반으로 RESTFul API 서버를 구현해보자.

---

## express + MVC

> 참고 글 : [MVC 패턴](https://psmin1994.github.io/posts/mvc/)

참고 글을 토대로 MVC 패턴을 활용해 구현해보겠습니다.

먼저 server 폴더를 만들고 터미널에 `npm init -y`을 실행하여 npm을 사용할 수 있는 초기 환경을 설정합니다.

View 부분으로는 추후에 React로 따로 구현할 예정이므로 **routes**, **controllers**, **models** 먼저 구현하겠습니다.

---

### 폴더 구성

> server
>
> > scraping
> >
> > src
> >
> > > config
> > >
> > > controllers
> > >
> > > models
> > >
> > > routes
> >
> > app.js

![mvc-logic](/assets/img/mvc-logic.png)

---

## app.js

> 참고 글 : [Express 사용법](https://psmin1994.github.io/posts/Express/)

`app.js` 파일을 생성한 후 router, middleware 등을 등록한 후 서버를 실행합니다.

```js
import express from "express";
import bodyParser from "body-parser";
import movieRouter from "./src/routes/movie.route.js";

const app = express();

let dirname = import.meta.dirname;

app.set("port", process.env.PORT || 5000);

// middleware
app.use(express.static(`${dirname}/src/public`));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));

// router
app.use("/movie", movieRouter);

// error handler
app.use((err, req, res, next) => {
  console.log("error : \n", err);

  res.status(500).json({
    msg: "Server Error!!",
    error: err,
  });
});

app.listen(process.env.PORT, () => {
  console.log("Server on Port ", process.env.PORT);
});
```

---

## router 설정

routes 폴더에 `movie.route.js` 파일을 생성한 후 express router를 작성합니다.

route는 사용자가 요청한 **URL 경로와 HTTP 메소드**에 따라 정해진 **controller로 연결**해줍니다.

즉, **요청에 대한 응답을 결정**하는 역할을 합니다.

> 유효성 검사는 Route에서 진행할 예정

```js
import express from "express";
import ctrl from "../controllers/movie.controller.js";

const router = express.Router();

router.get("/", ctrl.getMovie); // 영화 목록 불러오기
router.get("/:id", ctrl.getMovieById);
router.get("/movie_genre/:id", ctrl.getGenreByMovieId);
router.get("/movie_actor/:id", ctrl.getActorByMovieId);
router.get("/movie_director/:id", ctrl.getDirectorByMovieId);

router.get("/genre/:id", ctrl.getMovieByGenreId);

export default router;
```

---

## controller 설정

**controllers 폴더**에 `movie.controller.js` 파일을 생성한 후 작성하겠습니다.

controller는 model과 view의 중간다리 역할로 **model이 가져온 데이터를 가공하여 view에 전달**하거나 **view가 보낸 데이터를 model에 전달하는 역할**을 합니다.

즉, **작업할 model과 렌더링할 view를 정의**하는 역할을 합니다.

```js
import movieStorage from "../models/movie.model.js";

export default {
  getMovie: async (req, res) => {
    const data = await movieStorage.getMovie();

    res.json(data);
  },

  getMovieById: async (req, res) => {
    let movieId = req.params.id;

    const data = await movieStorage.getMovieById(movieId);

    res.json(data);
  },

  getGenreByMovieId: async (req, res) => {
    let movieId = req.params.id;

    const data = await movieStorage.getGenreByMovieId(movieId);

    res.json(data);
  },

  getActorByMovieId: async (req, res) => {
    let movieId = req.params.id;

    const data = await movieStorage.getActorByMovieId(movieId);

    res.json(data);
  },

  getDirectorByMovieId: async (req, res) => {
    let movieId = req.params.id;

    const data = await movieStorage.getDirectorByMovieId(movieId);

    res.json(data);
  },

  getMovieByGenreId: async (req, res) => {
    let genreId = req.params.id;

    const data = await movieStorage.getMovieByGenreId(genreId);

    res.json(data);
  },
};
```

---

## model 설정

models 폴더에 `movie.model.js` 파일을 생성한 후 작성해보겠습니다.

Mysql 연동은 `mysql2/promise 모듈`을 사용해 커넥션 풀을 생성하고 연동했습니다.

해당 코드는 `config 폴더`에 따로 빼주고 import해서 사용했습니다.

---

```js
// src/config/dpPool.js

import mysql from "mysql2/promise";
import dotenv from "dotenv";

// dotenv는 현재 Node.js 프로세스의 루트 디렉토리
// console.log(process.cwd());
dotenv.config({ path: "./server/.env" });

// 새로운 Pool 생성
let pool = await mysql.createPool({
  host: process.env.DB_HOST,
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  database: process.env.DB_DATABASE,
  port: process.env.DB_PORT,
  connectionLimit: process.env.DB_CONN_LIMIT,
  waitForConnections: true,
  queueLimit: 0,
  keepAliveInitialDelay: 10000, // 0 by default.
  enableKeepAlive: true, // false by default
});

// getConnection을 사용하여 Connection을 가져오는 함수
const getConnection = async () => {
  try {
    const connection = await pool.getConnection(); // getConnection()은 Promise를 반환합니다.
    return connection;
  } catch (err) {
    console.error("getConnection error:\n", err);

    if (err.code === "PROTOCOL_CONNECTION_LOST") {
      console.log("Connection lost. Reconnecting...");

      try {
        // 연결이 끊긴 경우 재시도
        const newConnection = await pool.getConnection();

        return newConnection;
      } catch (reConnErr) {
        console.error("Reconnection failed:", reConnErr);

        throw reConnErr; // 재시도도 실패하면 에러를 다시 throw하여 상위로 전파
      }
    } else {
      throw err; // 다른 에러는 그대로 상위로 전파
    }
  }
};

export default getConnection;
```

---

```js
// ./src/models/movie.model.js

import getConnection from "../config/dbPool.js";

class movieStorage {
  static async getMovie() {
    try {
      const conn = await getConnection();

      const [rows] = await conn.query(
        "SELECT movie_id, movie_nm, DATE_FORMAT(open_date, '%Y-%m-%d') AS open_date, showtime, poster FROM movie"
      );

      conn.release();

      return rows;
    } catch (err) {
      throw err;
    }
  }

  static async getMovieById(movieId) {
    try {
      const conn = await getConnection();

      const [rows] = await conn.query(
        "SELECT movie_nm_en, nation, summary, still_cut FROM movie WHERE movie_id = ?",
        [movieId]
      );

      conn.release();

      return rows;
    } catch (err) {
      throw err;
    }
  }

  static async getGenreByMovieId(movieId) {
    try {
      const conn = await getConnection();

      const [rows] = await conn.query(
        "SELECT g.name FROM genre g LEFT JOIN movie_and_genre mNg ON (g.genre_id = mNg.genre_id) WHERE mNg.movie_id = ?",
        [movieId]
      );

      conn.release();

      return rows;
    } catch (err) {
      throw err;
    }
  }

  static async getActorByMovieId(movieId) {
    try {
      const conn = await getConnection();

      const [rows] = await conn.query(
        "SELECT a.name, a.profile FROM actor a LEFT JOIN movie_and_actor mNa ON (a.actor_id = mNa.actor_id) WHERE mNa.movie_id = ?",
        [movieId]
      );

      conn.release();

      return rows;
    } catch (err) {
      throw err;
    }
  }

  static async getDirectorByMovieId() {
    try {
      const conn = await getConnection();

      const [rows] = await conn.query(
        "SELECT d.name, d.profile FROM director a LEFT JOIN movie_and_director mNd ON (d.director_id = mNd.director_id) WHERE mNd.movie_id = ?",
        [movieId]
      );

      conn.release();

      return rows;
    } catch (err) {
      throw err;
    }
  }

  static async getMovieByGenreId(genreId) {
    try {
      const conn = await getConnection();

      const [rows] = await conn.query(
        "SELECT m.movie_id, m.movie_nm, m.open_date, m.showtime, m.poster FROM movie m LEFT JOIN movie_and_genre mNg ON (m.movie_id = mNg.movie_id) WHERE mNg.genre_id = ?",
        [genreId]
      );

      conn.release();

      return rows;
    } catch (err) {
      throw err;
    }
  }
}

export default movieStorage;
```

---

## 응답 테스트

1. 전체 영화에 대한 기본 정보 가져오기

   ![postman-test-01](/assets/img/movie-postman-test-01.png)

2. id 값으로 특정 영화 상세 정보 가져오기

   ![postman-test-02](/assets/img/movie-postman-test-02.png)
