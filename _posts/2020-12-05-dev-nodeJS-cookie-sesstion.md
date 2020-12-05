---  
layout: post
title: nodeJS - 쿠키 / 세션
categories: dev
tags: nodeJS
comments: true
---

쿠키 : 서버와 클라이언트간 통신함에 있어 서비스 지속성(로그인 등)을 위한 목적으로 클라이언트 측에 저장하는 작은 기록 정보.

- 서버에 직접 쿠키를 만들어 요청자의 브라우저에 넣기.

```js
const http = require('http');

// createServer(콜백 함수)dptjsms req 객체에 담겨져 있는 쿠키 가져옴.
http.createServer((req, res) => {
    // req.headers.cookie에 쿠키가 들어 있음
    // req.headers는 요청의 헤더를 의미.
    // 쿠키는 요청의 헤더를 통해 이동.
    console.log(req.url, req.headers.cookie);
    // 쿠키는 문자열 형식으로 존재. 
    // 응답의 헤더에 쿠키를 기록해야 하므로 res.writeHead 메서드 사용
    // 'Set-Cookie' : '값'은 값을 쿠키에 저장하라는 의미
    res.writeHead(200, { 'Set-Cookie': 'mycookie=test'});
    res.end('Hello Cookie');
})
    .listen(8083, () => {
        console.log('8083 포트에서 서버 대기 중!')
    });

// 8083 포트에서 서버 대기 중!
// / undefined
// /favicon.ico mycookie=test
```

- 사용자를 식별하는 방법

```js
const http = require('http');
const fs = require('fs').promises;
const url = require('url');
const qs = require('querystring');

// 변수 명 pasrseCookies 함수로 문자열을 객체로 변경
const parseCookies = (cookie = '') =>
  cookie
    .split(';')
    .map(v => v.split('='))
    .reduce((acc, [k, v]) => {
      acc[k.trim()] = decodeURIComponent(v);
      return acc;
    }, {});

http.createServer(async (req, res) => {
  const cookies = parseCookies(req.headers.cookie);

  // 주소가 /login으로 시작하는 경우
  if (req.url.startsWith('/login')) {
    //  url과 querystring 모듈로 각각 주소와 주소에 딸려오는 query 분석
    const { query } = url.parse(req.url);
    const { name } = qs.parse(query);
    const expires = new Date();
    // 쿠키 유효 시간을 현재시간 + 5분으로 설정
    expires.setMinutes(expires.getMinutes() + 5);
    // 302(리다이렉트) 응답 코드와 리다이렉트 주소 입력
    // 브라우저는 이 응답 코드를 보고 페이지를 해당 주소로 리다이렉트.
    res.writeHead(302, {
        // 헤더더에는 한글을 설정할 수 없으므로 name 변수를 encodeURIComponent() 메서드로 인코딩
        // 또한 Set-Cookie 값으로는 제한 된 코드만 들어가야하므로 줄바꿈을 넣으면 안된다.
      Location: '/',
      'Set-Cookie': `name=${encodeURIComponent(name)}; Expires=${expires.toGMTString()}; HttpOnly; Path=/`,
    });
    res.end();

  // 그 외의 경우(/로 접속 했을 때) 우선 쿠키가 있는지 없는지 확인
  // name이라는 쿠키가 있는 경우
  } else if (cookies.name) {
    res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
    // 쿠키가 있따면 로그인한 상태로 간주하여 ~님 안녕하세요 전달.
    res.end(`${cookies.name}님 안녕하세요`);
    // 쿠키가 없다면
  } else {
    //  로그인 할 수 있는 페이지 보내기.
    // 쿠키가 없으므로 cookie2.html 전송
    try {
      const data = await fs.readFile('./cookie2.html');
      res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
      res.end(data);
    } catch (err) {
      res.writeHead(500, { 'Content-Type': 'text/plain; charset=utf-8' });
      res.end(err.message);
    }
  }
})
  .listen(8084, () => {
    console.log('8084번 포트에서 서버 대기 중입니다!');
  });
```

Set-Cookit로 쿠키를 설정 할때 만료 시간(Expires)과 HttpOnly, Path 같은 옵션을 부여했다. 쿠키 설정 시 각종 옵션을 넣을 수 있으며, 옵션 사이에 세미콜론(;)을 써서 구분하면 된다. 쿠키에는 한글과 줄바꿈 등이 들어가면 안된다. 한글인 경우 encodeURIComponent로 감싸서 넣는다.

- 쿠키명=쿠키값 : 기본적인 쿠키의 값. mycookie=test 또는 name=CREAMer 등으로 설정.
- Expires=날짜: 만료 기한. 이 기한이 지나면 쿠키 제거. 기본 값은 클라이언트가 종료될 때 까지.
- Max-age=초: Expires(만료 기한)과 비슷하지만 날짜 대신 초 입력 가능. 해당 초가 지나면 쿠키가 제거. Expires보다 우선시
- Domain=도메인명 : 쿠키가 전송될 도메인 지정. 기본 값은 현재 도메인
- Path=URL : 쿠키가 전송될 URL을 지정. 기본값은 '/'이고, 이 경우 모든 URL에서 쿠키 전송 가능
- Secure : HTTPS일 경우에만 쿠키 전송
- HttpOnly : 설정 시 자바스크립트에서 쿠키 접근 불가. 쿠키 조작 방지하기 위해 설정


- 민감한 개인정보를 쿠키에 넣어두는 것은 적절하지 못하다. 위 코드를 다음과 같이 변경하여 서버가 사용자 정보를 관리하도록 만들어야한다.

sesstion.js

```js
const http = require('http');
const fs = require('fs').promises;
const url = require('url');
const qs = require('querystring');

const parseCookies = (cookie = '') =>
  cookie
    .split(';')
    .map(v => v.split('='))
    .reduce((acc, [k, v]) => {
      acc[k.trim()] = decodeURIComponent(v);
      return acc;
    }, {});

const session = {};

http.createServer(async (req, res) => {
  const cookies = parseCookies(req.headers.cookie);
  if (req.url.startsWith('/login')) {
    const { query } = url.parse(req.url);
    const { name } = qs.parse(query);
    const expires = new Date();
    expires.setMinutes(expires.getMinutes() + 5);

    // cookie2.js와 다른 부분.
    // 쿠키에 이름을 담아서 보내는 대신 uniqueInt라는 숫자 값을 보낸다.
    const uniqueInt = Date.now();
    // 사용자의 name과 expires는 uniqueInt 속성명 아래에 있는 session이라는 객체에 대신 저장
    session[uniqueInt] = {
      name,
      expires,
    };
    res.writeHead(302, {
      Location: '/',
      'Set-Cookie': `session=${uniqueInt}; Expires=${expires.toGMTString()}; HttpOnly; Path=/`,
    });
    res.end();
  // 세션쿠키가 존재하고, 만료 기간이 지나지 않았다면
  } else if (cookies.session && session[cookies.session].expires > new Date()) {
    res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
    res.end(`${session[cookies.session].name}님 안녕하세요`);
  } else {
    try {
      const data = await fs.readFile('./cookie2.html');
      res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
      res.end(data);
    } catch (err) {
      res.writeHead(500, { 'Content-Type': 'text/plain; charset=utf-8' });
      res.end(err.message);
    }
  }
})
  .listen(8085, () => {
    console.log('8085번 포트에서 서버 대기 중입니다!');
  });
```
실제 배포용 서버에서는 세션을 위와 같이 변수로 저장하지는 않는다. (서버가 멈추거나 시작되면 메모리에 저장된 변수가 초기화되기 때문 또한 서버의 메모리가 부족하면 세션을 저장하지 못하는 문제도 생김. 그래서 보통 레디스(Redis)나 맴캐시드( Memcached)같은 데이터 베이스에 넣어둔다.)

**절대 위 코드를 실제 서비스에 사용해서는 안된다. 보안상 매우 취약하다.**

세션 : 서버에 사용자 정보를 저장하고 클라이언트와는 세션 아이디로만 소통. 세션 아이디는 꼭 쿠키를 사용해서 주고받지 않아도 된다. 

세션 쿠키 : 세션을 위해 사용하는 쿠키