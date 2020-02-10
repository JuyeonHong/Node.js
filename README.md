# node.js 

## npm start
- npm(Node Package Manager)  
	node.js로 만들어진 package(module)을 관리해주는 툴
- package.json으로 package(module) 관리하기  
	프로젝트에서 사용하는 외부모듈이 많아질 경우 module마다 버전을 관리하기 힘들고,  
	module개수만큼 npm install 을 입력해야하는 불편한 상황이 발생.  
	이런 문제를 해결하기 위해 설치된 module을 list화해서 관리할 수 있는 파일이 **package.json**.
	* package.json 생성  
	`npm init`
	* package.json으로 module 한 번에 다운로드하기  
	`npm install`
- package.json에서 중요한 것   
	* script: run 명령어를 통해서 실행할 것들을 적어둠
	* dependencies: 설치할 모듈들을 의미
		`npm install -g webpack` 설치를 통해 자동으로 기록할 수 있음
-  Express  
	웹 및 모바일 애플리케이션을 위한 일련의 기능을 제공하는 Node.js 웹 애플리케이션 프레임워크
- 기본예제  
- ```
	var express = require('express');
	var app = express();
	
	// 주소가 '/' 최상위 url로 들어오면 Hello hello 내보냄
	app.get('/', function(req,res) {
	    res.send("<h1>hello hello</h1>");
	})
	
	// 3000번 포트로 들어오면 열어주는 부분
	app.listen(3000, function() {
	    console.log('example app listening on port 3000');
	})
	```

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
