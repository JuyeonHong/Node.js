# node.js 

## node.js in mySQL
### mySQL 시작하기
1. ```mysql.server start```
2. ```mysql -u root(계정이름) -p```  
3. 로그인 성공!

### Database 
- 데이터베이스 조회  
```show databases;```  

- 데이터베이스 만들기  
```ex) create database myDB;``` 
	
- 데이터베이스 사용  
```ex) use myDB;```

### Table
- 테이블 조회  
```show tables;```  
	 
- 테이블 만들기  
```
create table user(
	email char(20),
    	name char(20),
    	pw char(20),
    	primary key(email)
);
```  
			
- 테이블에 데이터 넣기  
```insert into user ('sql@naver.com', 'mysql', '1234');```  
 =  
```insert into user (email, name, pw) values ('sql@naver.com', 'mysql', '1234');```
	 
### node.js
```npm install mysql --save (또는 --g)```

```
var mysql = require("mysql");
var connection = mysql.createConnection({
      host: "localhost",
      user: "root",
      password: "password",
      database: "myDB"
});
connection.connect();
```

## Routing 모듈화
### 하나의 .js를 별도의 라우터 기반으로 모듈을 만들어 뺄 수 있다.

#### Ex) app.js에서 main.js를 로딩해서 사용하기


##### main.js
```
var express = require("express");
var app = express();
// 1
var router = express.Router();
// 2
var path = require("path");

router.get("/", function(req, res) {
    console.log("main js loaded");
    // 2
    res.sendfile(path.join(__dirname, "../public/main.html"));
});
//3
module.exports = router;
```

1. 라우터 사용하는 부분
2. **path**를 사용하면 상대경로로 쉽게 이동가능하다.  
	위의 코드같은 경우, **__dirname(현재경로)**에서 **..(한 단계 위)**로 올라가서 **/public.main.html** 경로를 쓰겠다라는 뜻.
3. export해주겠다는 뜻


##### app.js

```
// 1
var main = require("./router/main");
// 2
app.use("/main", main);
```
1. 모듈 불러오기  
	**main.js**를 **app.js**에 로딩해서 사용하는 부분
2. 모듈 정의  
	**/main**으로 들어오면 1에서 선언한 **main**을 쓰라는 뜻.  
	즉, **main**으로 들어올 경우, **main**라우터 정보로 들어가서 필요한 처리를 할 수 있다.  
	
