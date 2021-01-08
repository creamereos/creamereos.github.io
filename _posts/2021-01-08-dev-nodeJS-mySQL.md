---  
layout: post
title: nodeJS - MySQL
categories: dev
tags: node
comments: true
---

- 데이터베이스: 관련성을 가지며 중복이 없는 데이터들의 집합. 서버 종료 여부와 상관없이 데이터를 지속적으로 사용 가능. 서버에 데이터베이스를 올려 여러사람이 동시에 사용 가능. 사람들에게 각각 다른 권한(읽기,쓰기 등)을 줄 수 도 있다.
- DBSM(DataBase Management System): 데이터베이스 관리 시스템
- RDBSM(Relational DataBase Management System): 관계형 데이터베이스 관ㄹ리 시스템. SQL 언어를 사용하여 데이터 관리

### MySQL
- MySQL: SQL 언어를 사용하는 관계형 데이터베이스 관리 시스템

설치
```
$ brew install mysql
$ brew services start mysql
$ mysql_secure_installation
```

접속
```
$ mysql -h localhost -u root -p
Enter password : [비밀번호 입력]
```

- 워크벤치(Workbench) : MySQL GUI 툴

설치

```
$ brew cask install mysqlworkbench
```

### 데이터 베이스 생성하기

콘솔에서 MySQL 로그인 후 CREATE SCHEMA 데이터베이스 이름

```
mysql> CREATE SCHEMA nodejs DEFAULT CHARACTER SET utf8;
```

스키마(SCHEMA)는 데이터베이스와 같은 개념.
DEFAULT CHARACTER SET utf8을 작성하여 한글 사용 가능.
SQL 구문 입력시 마지막에 세미콜론(;)을 붙여야 실행.
CREATE SCHEMA 같이 MySQL이 기본적으로 알고 있는 구문을 예약어라고 부른다. 예약어는 소문자로 써도 상관없지만 구분을 위해 대문자로 사용한다.

```
mysql> use nodjs;
```

앞으로 nodejs 데이터베이스를 사용하겠다는 것을 MySQL에 알리기

### 테이블 생성하기

- 테이블 : 데이터가 들어갈 수 있는 틀. 테이블에 맞는 데이터만 들어갈 수 있다. 

테이블 생성 명령어 : **CREATE TABLE 데이터베이스명.테이블명(컬럼 자료형 옵션,컬럼 자료형,...) 컬럼 관련 설정**  
데이터 베이스 내에 테이블을 생성.

```
CREATE TABLE nodejs.users(
    id INT NOT NULL AUTO_INCREMENT,
    name VARCHAR(20) NOT NUll,
    age INT UNSIGNED NOT NULL,
    married TINYINT NOT NULL,
    commnet TEXT NULL,
    create_at DATETIME NOT NULL DEFAULT now(),
    PRIMARY KEY(id),
    UNIQUE INDEX name_UNIQUE (name ASC))
    COMMENT = '사용자 정보'
    DEFAULT CHARACTER SET = utf8
    ENGINE = InnoDB;
```

위 코드는 한줄로 적을 수 있지만 가독성을 위해 엔터로 줄바꿈. 세미콜론(;)을 입력하기전까지는 실행되지 않음.

### 컬럼의 자료형

컬럼의 옆에 붙는다.

- INT : 정수. 
- FLOAT, DOUBLE : 소수.
- VARCHAR(숫자) : 가변 길이. ex) VARCHAR(10)인 경우 길이가 0~10인 문자열을 넣을 수 있다.
- CHAR(숫자) : 고정 길이. ex)CHAR(10)인 경우 반드시 길이가 10인 문자열만 넣어야하고 그 길이보다 짧으면 스페이스바로 채워진다.
- TEXT : 긴 글을 저장 할 때 사용. 수백자 이내의 문자열은 보통 VARCHAR 사용. 이보다 길면 TEXT 사용
- TINYINT : -128~127까지의 정수를 저장할 때 사용. 1 또는 0만 저장한다면 Boolean과 같은 역할을 할 수 있다.
- DATETIME : 날짜와 시간에 대한 정보
- DATE : 날짜에 대한 정보
- TIME : 시간에 대한 정보

### 옵션

컬럼의 옆에 자료형 옆에 붙는다.

- NULL : 빈칸 허용. comment 컬럼만 NULL
- NOT NULL : 빈칸 허용하지 않음. 나머지 컬럼은 모두 NOT NUL
- AUTO_INCREMNET : 데이터 입련 순으로 숫자를 자동으로 올리는 옵션. 예를 들어 creamer의 데이터를 넣으면 MySQL은 알아서 id로 1번을 부여. 다음에 terry의 데이터를 넣으면 자동으로 id 2번을 부여.
- UNSIGNED : 숫자 자료형에 적용되는 옵션. 음수는 무시하고 양수만 저장 가능. (ex 나이처럼 음수가 나올 수 없는 컬럼에 설정)
- ZEROFILL : 숫자의 자릿수가 고정되어있을 때 사용. 비어있는 자리에 모두 0을 넣는다. 예를 들어 INT(4)인데 숫자 1을 넣었다면 0001이 된다.
- DEFAULT now() : 데이터 베이스 저장 시 해당 컬럼에 값이 없다면 MySQL이 기본 값을 대신 넣는다. now()는 현재 시간을 넣으라는 뜻. = DEFAULT CURRENT_TIME.
- PRIMARY KEY : 해당 컬럼이 기본 키인 경우 사용. 기본 키란 row를 대표하는 고유한 값을 의미. (다른 데이터와 겹치지 않는 고유한 컬럼에 적용. ex: id, 주민등록번호, 학번 등)
- UNIQUE INDEX : 해당 값이  고유해야하는지에 대한 옵션. (PRIMARY KEY는 자동으로 UNIQUE INDEX를 포함)

### 컬럼 관련 설정

- COMMENT : 보충 설명 (필수 아님)
- DEFAULT CHARACTER SET utf8 : 한글 사용 시 필수 설정
- ENGINE : MyISAM, InnoDB 주로 사용

### 생성한 테이블 확인

DESC 테이블명

```
mysql> DESC users;
```

### 생성한 테이블 제거

DROP TABLE 테이블명

```
mysql> DROP TABLE users;
```

### 댓글 저장 테이블 만들기

- 컬럼 : id / commenter / comment / create_at

```
mysql > 
    CREATE TABLE nodejs.comments (
    id INT NOT NULL AUTO_INCREMENT,
    commenter INT NOT NULL,
    comment VARCHAR(100) NOT NULL,
    create_at DATETIME NOT NULL DEFAULT now(),
    PRIMARY KEY(id),
    INDEX commenter_idx (commenter ASC),
    CONSTRAINT commenter 
    FOREIGN KEY (commenter)
    REFERENCES nodejs.users (id)
    ON DELETE CASCADE
    ON UPDATE CASCADE)
    COMMENT = '댓글'
    DEFAULT CHARSET=utf8mb4
    ENGINE=InnoDB;
```

- FOREIGN KEY : 다른 테이블의 기본 키를 저장하는 컬럼.

CONSTRAINT 제약조건명 FOREIGN KEY 컬럼명 REFERENCES 참고하는 컬럼명 으로 FOREIGN KEY를 지정 할 수 있다.

ex) CONSTRAINT commenter FOREIGN KEY (commenter) REFERENCES nodejs.users (id)

comments 테이블에서는 commenter 컬럼과 users 테이블의 id 컬럼을 연결.

- INDEX commenter_idx (commenter ASC) : id는 다른 테이블의 기본 키이므로 commenter 컬럼에 인덱스도 사용.

- ON DELETE CASCADE / ON UPDATE CASCADE : 사용자 정보가 수정되거나 삭제되면 그것과 연결된 댓글 정보도 같이 수정하거나 삭제 한다는 뜻. 이 방법을 통해 데이터 불일치 현상이 나타나지 않는다.

### 생성한 테이블 전체 확인

```
mysql> SHOW TABLES;
```