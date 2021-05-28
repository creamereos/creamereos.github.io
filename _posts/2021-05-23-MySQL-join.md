--- 
layout: post  
title: MySQL 다중 테이블 연산(JOIN, UNION, SubQeury)  21.05.23
subtitle: 
categories: dev
tags: 
comments: true  
--- 

![join](/assets/img/post/2021-05-22-17-19-07.png)

- JOIN : 데이터베이스 내의 여러 테이블에서 가져온 레코드를 조합하여 하나의 테이블이나 결과 집합으로 표현

```
<!-- 조건에 따라 첫번째테이블과 두번째테이블의 레코드를 합친 테이블 만듬 -->
첫번째테이블이름
JOIN 두번째테이블이름
ON 조건

<!-- Ex code1 -->
SELECT *
FROM Reservation
JOIN Customer
ON Reservation.Name = Customer.Name;

<!--EX code1과 같은 결과-->
SELECT *
FROM Reservation, Customer
WHERE Reservation.Name = Customer.Name;

<!-- 별칭 사용 -->
SELECT *
FROM Reservation AS r, Customer AS c
WHERE r.Name = c.Name;
```

- LEFT JOIN : 첫 번째 테이블(기준)에 두 번째 테이블 조합

```
첫번째테이블이름
LEFT JOIN 두번째테이블이름
ON 조건

<!-- ex code1 -->
SELECT *
FROM Reservation
LEFT JOIN Customer
<!-- ON : JOIN 전의 조건 -->
ON Reservation.Name = Customer.Name
<!-- WHERE : JOIN 후의 조건 -->
WHERE ReserveDate > '2016-02-01';
```

### ON vs WHERE

ON : JOIN 을 하기 전 조건 (=ON 조건으로 필터링이 된 레코드들간 JOIN이 이뤄진다)

WHERE : JOIN 을 한 후 조건 (=JOIN을 한 결과에서 WHERE 조건절로 필터링이 이뤄진다)

##### 3개 이상의 테이블 JOIN

```
첫번째테이블이름
JOIN 두번째테이블이름
ON 조건
JOIN 세번째테이블이름
ON 조건
...
JOIN n번째테이블이름
ON 조건

<!-- Ex code -->
SELECT *
FROM Reservation
JOIN Customer
ON Reservation.Name = Customer.Name
JOIN Date
ON Customer.DateId = Date.id
```

### Union : 여러 개의 SELECT 문의 결과를 하나의 테이블이나 결과 집합으로 표현

```
<!-- 중복 레코드 제거 -->
SELECT 필드이름
FROM 테이블이름
UNION
SELECT 필드이름
FROM 테이블이름

<!-- 중복 레코드 제거안함 -->
SELECT 필드이름
FROM 테이블이름
UNION ALL
SELECT 필드이름
FROM 테이블이름
```

### SubQeury : 다른 쿼리 내부에 포함되어 있는 SELETE 문을 의미. 서브쿼리는 반드시 괄호()로 감싸야 함.

서브쿼리의 특징 및 장점

1. 서브쿼리는 쿼리를 구조화시키므로, 쿼리의 각 부분을 명확히 구분할 수 있게 해줍니다.

2. 서브쿼리는 복잡한 JOIN이나 UNION과 같은 동작을 수행할 수 있는 또 다른 방법을 제공합니다.

3. 서브쿼리는 복잡한 JOIN이나 UNION 보다 좀 더 읽기 편합니다.

하지만 서브쿼리에서 사용된 테이블이나 그 결과 집합은 수정할 수 없다.

```
SELECT ID, ReserveDate, RoomNum
FROM Reservation
WHERE Name IN (SELECT Name 
               FROM Customer
               WHERE Address = '서울')

<!-- 위 쿼리를 JOIN 문으로 변경 -->
SELECT r.ID, r.ReserveDate, r.RoomNum
FROM Reservation AS r, Customer AS c
WHERE c.Address = '서울' AND r.Name = c.Name;

<!-- FROM 절에서 서브쿼리 사용 -->
SELECT Name, ReservedRoom
FROM (SELECT Name, ReserveDate, (RoomNum + 1) AS ReservedRoom
      FROM Reservation
      WHERE RoomNum > 1001) 
AS ReservationInfo;
```

참고 : 
http://tcpschool.com/mysql/mysql_multipleTable
https://coding-factory.tistory.com/87
https://developyo.tistory.com/121