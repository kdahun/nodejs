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
