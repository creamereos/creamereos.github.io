---  
layout: post  
title: RESTful API 21.05.07
subtitle: 
categories: dev
tags: node
comments: true  
--- 

# RESTful API

- REpresentational State Transfer : 표현 상태 전달

- Web을 망가뜨리지 않고 HTTP를 개선하는 방법

- 웹서비스를 만드는데 사용되는 제약(constraint) 모음

### [웹서비스를 만드는데 사용되는 제약 조건]

클라이언트 -- 요청(HTTP와 API) -> 서버

리소스 마다 서로 다른 API 규칙을 가지고 있다. API 규칙을 통일하기 위해 RESTful API가 필요하다.

1.Client-Server (HTTP)
서버와 클라이언트가 각각 독립적으로 작성되야한다. 서로 다른 클라이언트와 서버가 호환가능하도록

2.Stateless (HTTP)
요청은 상태를 유지하지 않기 때문에 매번 요청에 필요한 모든 정보를 넣어줘야한다.

3.Cacheable (HTTP)
계속 Stateless하게 정보를 주고 받으면 데이터가 커지기 때문에 특정 정보를 서버에 저장

4.Uniform interface 
API 작성 규칙을 통일.

5.Layerd system (HTTP)
서버(서버의 서버: 데이터베이스 서버)가 어떤 방식으로 구성되고 작동되는지 몰라도 API를 사용할 수 있게 작성

6.Code on demand (JS)
자바스크립트 wkfTjtj tkdyd

1,2,3,5,6은 대부분 코드만 잘작성하면 해결. 4번을 해결하는 것이 RESTful API에 중요하다.

REST에서 정보의 가장 핵심적인 추상화는 리소스다. 이름 붙일 수 있는 정보면 어떤 것이든 리소스가 될 수 있다.

### Uniform interface 조건 

가이드 라인 : https://restfulapi.net/resource-naming/

1. 리소스를 나타내는 데 명사를 사용해라
4가지 대분류 : documnet, collection, store, controller

2. 일관성이 핵심
- 계층 구조를 나타낼 떄는 / 를 사용하라
- URI 끝에 / 를 붙이지마라
- URI의 가동성을 높이기 위해 띄어쓰기 말고 -를 사용하라 (_ 언더바 사용 금지)
- URI에 소문자를 사용해라
- 파일 확장자를 사용하지 마라 (파일 정보도 명시되기 때문에 확장자가 굳이 필요없다.)

3. CRUD 기능 이름은 URI에 사용하지마라
CRUD : POST, GET, PUT, DELETE를 URI 이름으로 사용하지마라

4. filter가 필요하면 query component를 사용해라

ex : ?region=USA&brand=eos

# 추가로 공부하면 좋은 자료>>><<>>

- restfulapi.net

- 그런 REST API로 괜찮은가 - Naver DEVIEW 2017
