# Node.js를 사용하여 웹 서버를 만들고 MySQL을 연결

## 1. 필요한 라이브러리 가져오기
```
const express = require("express");
const mysql = require("mysql2");
const bodyParser = require("body-parser");
const cors = require("cors");
```
- express : 간단하게 웹 서버를 만들 수 있게 도와주는 라이브러리
- mysql : MySQL과 연결하고 통신
- bodyParser : 클라이언트가 보낸 데이터를 쉽게 처리
- cors : 다른 도메인에서 온 요청을 허용할 수 있게 해주는 라이브러리. 이를 통해 다른 웹사이트에서도 이 서버에 요청할 수 있다.

## 2. 웹 서버 만들기
```
const app = express();
```
express 라이브러리를 사용하여 app이라는 웹 서버 객체를 만든다. 이 app 객체를 사용하여 서버의 동작을 정의할 수 있다.

## 3. 미들웨어 설정
```
app.use(bodyParser.json());
app.use(cors());
```
- app.use(bodyParser.json()) : 클라이언트가 서버로 JSON 형태의 데이터를 보낼 때, 이 데이터를 쉽게 사용할 수 있도록 변환
- app.use(cors()) : 다른 웹사이트나 도메인에서 이 서버에 접근할 수 있도록 허용

## 4. 데이터베이스 연결 설정
```
const pool = mysql.createPool({
  host : "202.31.147.126",
  user : "root",
  database : "videoData",
  password : "5159",
  waitForConnections : true,
  connectionLimit : 10,  // 동시에 최대 몇 개의 연결을 유지할지 설정
  queueLimit : 0, 
});
```

## 5. 검색 요청을 처리하는 엔드포인트 만들기
```
app.post("/search", async (req, res) => {
  const searchText = req.body.searchText;

  if (!searchText) {
    return res.status(400).send({ error: "검색 텍스트를 입력하세요." });
  }

  try {
    const promisePool = pool.promise();
    const query = `
      SELECT timestamp, subText, voiceText 
      FROM datatable 
      WHERE subText LIKE ? OR voiceText LIKE ?
    `;
    const [rows] = await promisePool.query(query, [
      `%${searchText}%`,
      `%${searchText}%`,
    ]);
    res.send(rows);
  } catch (error) {
    console.error("Error:", error.message);
    res.status(500).send({ error: "서버 오류가 발생했습니다." });
  }
});
```
여기서는 "/search"라는 URL로 클라이언트가 요청을 보내면 어떻게 처리할지 정의한다.
- app.post("search",...) : POST 방식으로 /search URL로 들어오는 요청을 처리한다.
- req.body.searchText : 클라이언트가 보낸 데이터 중에서 searchText라는 값을 가져온다.
- if(!searchText) : searchText가 없으면, 오류 메시지를 클라이언트에게 보낸다
- const promisePool = pool.promise() : 데이터베이스 연결을 사용하기 위해 promisePool을 설정
- const query = ... : 데이터베이에서 검색할 SQL 쿼리를 정의한다. 이 쿼리는 subText나 voiceText에 searchText가 포함된 모든 행을 찾는다.
- const[rows] = awit promisePool.query(query,[])






























