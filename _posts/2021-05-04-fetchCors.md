---  
layout: post  
title: CORS 21.05.04
subtitle: 
categories: dev
tags: node
comments: true  
--- 

fetch로 서버에 request 할 때 fetch 인자 url이 현재 접속 사이트와 다르다면 request가 실패 할 수 있다.

```js
try {
    await fetch('http://example.com');
}   catch(err) {
    console.log(err); 
}
// TypeError: Failed to fetch
```

실패한 이유는 origin 때문이다. 

- origin : 도메인, 프로토콜, 포트에 의해 결정

```
ex)
https://www.google.com/search?q=bitcoin#p1

protocol(==scheme) : https
subdomain : www
domain(==host) : google.com
path : search
query string : q=bitcoin
hasg fragment : p1
```

도메인(또는 서브도메인)이나 프로토콜, 포트가 다른 곳에 요청을 보내는 것을 CORS(Cross-Origin Request)라고 하며, 이 COR 요청을 보내려면 리모트 origin에서 특별한 헤더가 필요하다. 

위와 같은 정책을 CORS(Cross-Origin Request Sharing)라고 한다.

##### 즉, 오리진(도메인 || 서브 도메인 || 프로토콜 || 포트)이 다른 곳에 request를 보내는 경우 특별한 헤더가 필요.

### CORS가 필요한 이유

CORS는 해커로부터 인터넷을 보호하기 위해 만들어졌다. 과거에는 한 사이트의 스크립트에서 다른 사이트에 있는 컨텐츠에 접속할 수 없다는 제약(SOP:Single Origin Policy)이 있었다. 이는 간단하지만 인터넷 보안을 위한 강력한 규칙이었다. 

하지만 최근 웹에서 많은 기능이 필요하기 시작하면서 위 제약을 피하기 위한 방법들이 생겨났다.

최근 웹은 싱글 웹페이지 어플리케이션이라는 기술이 생겨서 하나의 클라이언트가 여러 곳에 있는 리소스를 활용할 필요가 생겼다. (거래소, 유튜브 API 활용 등)

### CORS 요청 기능 개요 

CORS는 새로운 HTTP 헤더를 추가하여 동작. GET을 제외한 HTTP method에  CORS는 브라우저가 요청을 OPTIONS method로 preflight(사전 전달) 하여 지원하는 method를 요청하고, 서버가 허락하면 실제 요청을 보내도록 요구한다. 또한 서버는 클라이언트에게 요청에 인증정보(쿠키, HTTPS 인증)를 함께 보내야한다고 알려 줄 수도있다.

### [CORS 시나리오 예제]

##### 1. 단순 요청 (Simple Reqeust)

- preflight가 필요하지 않다.

- 조건 : 
(method : GET || HEAD || POST) 
&& User agent가 기본 값 
&& (Content-Type : application/x-www-form-urlencoded || multipart/form-data || text/plain)

##### 2. 사전 요청 (Prefilght Request)

- 먼저 OPTIONS 메서드를 통해 다른 도메인의 리소스로 HTTP 요청을 보내 실제 요청이 전송하기에 안전한지 확인

```js
const defaultCorsHeader = {
  // 허용할 Origin(도메인) : 모든 도메인(*)
  'Access-Control-Allow-Origin': '*',
  // 허용할 method 
  'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
  // 허용할 헤더 
  'Access-Control-Allow-Headers': 'Content-Type, Accept',
  // preflight request를 허용할 시간(초)
  'Access-Control-Max-Age': 10
};
```

preflight 요청을 보내서 위 조건이 맞는지 확인