---  
layout: post
title: nodeJS - http / https
categories: dev
tags: node
comments: true
---

https : https 모듈은 웹 서버에 SSL 암호화를 추가. GET / POST 요청 시 오가는 데이터를 암호화화여 중간에 다른 사람이 요청을 가로채더라도 내용을 확인 할 수 없게 한다.

- http 서버 코드를 https 서버코드로 바꾸기

http.js

```js
const http = require('http');

http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
  res.write('<h1>Hello Node!</h1>');
  res.end('<p>Hello Server!</p>');
})
  .listen(8080, () => { // 서버 연결
    console.log('8080번 포트에서 서버 대기 중입니다!');
  });
```

https.js

```js
const https = require('https');
const fs = require('fs');

// 다른 것은 거의 비슷하지만 createServer 메서드가 인수를 2개 받는다.
https.createServer({
  // 1번째 인수 : 인증서와 관련된 옵션 객체 
  cert: fs.readFileSync('도메인 인증서 경로'),
  key: fs.readFileSync('도메인 비밀키 경로'),
  ca: [
    fs.readFileSync('상위 인증서 경로'),
    fs.readFileSync('상위 인증서 경로'),
  ],
}, 
    // 2번째 인수 : 서버 로직
    (req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
    res.write('<h1>Hello Node!</h1>');
    res.end('<p>Hello Server!</p>');
})
   //실제 서버에서는 80포트 대신 443 포트 사용
  .listen(443, () => {
    console.log('443번 포트에서 서버 대기 중입니다!');
  });
```

위 코드를 적용하려면 https 인증서 발급이 필요하다.

- http2 모듈: 웹의 속도 개선 (효율적 요청) + 보안

http2.js

```js
// https 대신 http2로 변경
const http2 = require('http2');
const fs = require('fs');

// creaateServer 대신 createSecureServer
http2.createSecureServer({
  cert: fs.readFileSync('도메인 인증서 경로'),
  key: fs.readFileSync('도메인 비밀키 경로'),
  ca: [
    fs.readFileSync('상위 인증서 경로'),
    fs.readFileSync('상위 인증서 경로'),
  ],
}, (req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
  res.write('<h1>Hello Node!</h1>');
  res.end('<p>Hello Server!</p>');
})
  .listen(443, () => {
    console.log('443번 포트에서 서버 대기 중입니다!');
  });
```