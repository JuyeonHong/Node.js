# node.js in mySQL

## mySQL 시작하기
1. ```mysql.server start```
2. ```mysql -u root(계정이름) -p```  
3. 로그인 성공!

## Database 
- 데이터베이스 조회  
```show databases;```  

- 데이터베이스 만들기  
```ex) create database myDB;``` 
	
- 데이터베이스 사용  
```ex) use myDB;```

## Table
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
	 
## node.js
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
