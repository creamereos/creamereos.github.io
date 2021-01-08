---  
layout: post
title: nodeJS - MySQL CRUD
categories: dev
tags: node
comments: true
---

CRUD(Create / Read / Update / Delete)

### Create

데이터를 생성하여 데이터베이스에 넣는 작업

INSERT INTO 테이블명 (컬럼1, 컬럼2, ...) VALUES (값1, 값2 ...)

```
mysql> INSERT INTO nodejs.users (name, age, married, comment) VALUES ('creamer', 30, 0, 'hi');
mysql> INSERT INTO nodejs.users (name, age, married, comment) VALUES ('terry', 30, 0, 'hello');
mysql> INSERT INTO nodejs.comments (commenter, comment) VALUES (1, 'Cash Rules Everything Around Me');
```

- 컬럼명 오타 발생시 변경 방법
ALTER TABLE 테이블명 CHANGE 기존컬럼명 변경할컬럼명 컬럼타입;

### Read

SELECT * FROM 테이블명

```
mysql> SELECT * FROM nodejs.users;
mysql> SELECT * FROM nodejs.comments;
```

- 특정 컬럼 조회하기

SELECT 컬럼명, 컬럼명.. FROM 테이블명

```
mysql> SELECT name, age FROM nodejs.users;
mysql> SELECT married FROM nodejs.users;
```

- WHERE : 특정 조건을 충족하는 데이터 조회

SELECT 컬럼명, 컬럼명.. FROM 테이블명 WHERE 조건

```
SELECT name, age FROM nodejs.users WHERE age > 30;
```

- WHERE AND : 여러 조건을 충족하는 데이터 조회

SELECT 컬럼명, 컬럼명.. FROM 테이블명 WHERE 조건 AND 조건;

```
SELECT name, age FROM nodejs.users WHERE age > 30 AND married = 0;
```

- WHERE OR : 여러 조건 중 하나라도 충족하는 데이터 조회

SELECT 컬럼명, 컬럼명.. FROM 테이블명 WHERE 조건 OR 조건;

```
SELECT name, age FROM nodejs.users WHERE age > 30 OR married = 0;
```

- ORDER BY 컬럼명 ASC||DESC : 오름차순 || 내림차순 정렬

```
SELECT id, name, age FROM nodejs.users ORDER BY age DESC
```

- LIMIT : 조회할 row 개수 설정. 

LIMIT 숫자

```
SELECT id, name, age FROM nodejs.users ORDER BY age DESC LIMIT 1;
```

- OFFSET : 건너 뛰기 (게시판의 페이지 네이션 기능 구현시 유용)

OFFSET 건너뛸 숫자

```
SELECT id, name, age FROM nodejs.users ORDER BY age DESC LIMIT 1 OFFSET 1;
```

### Update

데이터베이스에 있는 데이터 수정

UPDATE 테이블명 SET 컬럼명 = 바꿀 값 WHERE 조건;

```
mysql> UPDATE nodejs.users SET comment = '코멘트 변경' WHERE id = 2;
```

### Delete

데이터베이스에 있는 데이터 삭제

DELETE FROM 테이블명 WHERE 조건;

```
mysql> DELETE FROM nodejs.users WHERE id = 2;
```
