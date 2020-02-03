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
	
	
### Type
- int 타입 자리 수 지정하는 것 deprecated > ```그냥 자리수 지정 없이 int 타입으로 선언하면 될 것 같음 ```
- 문자열은 가변길이 타입인 ```varchar 사용```
- varchar의 길이 지정은 텍스트 문자 개수라고 생각하면 될 듯 > ```varchar(30): 30자리 텍스트를 저장하겠다```
- 긴 텍스트글은 text 타입 사용 > ```ex) 후기 글 저장할 때```
- 날짜는 datetime 아니면 date 타입 사용 > ```datetime: 2020-02-03 15:00:03 / date: 2020-02-03```

### 속성
- 빈 값이 오면 안되는 컬럼은 ```not null 옵션 넣기```
- Primary key에는 ```auto_increment 추가``` 

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
	위의 코드같은 경우, **__dirname(현재경로)** 에서 **..(한 단계 위)** 로 올라가서 **/public.main.html** 경로를 쓰겠다라는 뜻.
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
	

## 필요할 것 같은 모듈
- 비밀번호 암호화 > ``` bcrypt/bcryptjs```

## 기타
- Node.js는 비동기 처리가 기본
- 동기 처리가 필요한 부분은 ```async await promise 사용하기```
```
ex)
let user = {};

db.query(`SELECT * FROM user WHERE user_id = ?`, [1], function(err, rows) {
 user.name = rows[0].name;
 user.age = rows[0].age;
 .
 .
 .
 
})

res.json({
 status: 'success',
 user: user
})

// 위와 같이 db에서 조회 후 user 데이터를 객체에 넣고 그 객체를 json으로 넘겨줄 때, user 데이터가 먼저 구성되어야 하므로 동기 처리가 필요하다.
// db.query의 콜백함수 안에 json 응답을 넣어도 되지만 db.query가 여러 번 실행되는 경우에는 콜백지옥에 빠질 수 있다.
// 따라서 async await promise를 사용하도록 해보자..

```
- 사용자로부터 입력받는 모든 데이터는 항상 오염될 수 있다는 걸 명심하자. ``` validation 중요 !!! ```
```
front end: 사용자 편의를 위해
back end: DB 오염방지(?)와 보안을 위해
```
