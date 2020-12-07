---  
layout: post
title: nodeJS - REST와 라우팅
categories: dev
tags: node
comments: true
---

서버에 요청을 보낼 때 주소를 통해 요청의 내용 표현. (주소가 /inde.html이면 서버의 index.html을 보내달라는 뜻) 요청의 내용이 주소를 통해 표현되므로 서버가 이해하기 쉬운 주소를 사용해야한다. 

**REST(REpresentational State Transfer)** : 서버의 자원을 정의하고 자원에 대한 주소를 지정하는 방법. 

**[Request Method]**
- GET : **서버의 자원을 가져올 때** 사용. request의 본문에 데이터를 넣지 않는다. 데이터를 서버에 보내야한다면 쿼리스트링 사용. 브라우저에서 캐싱(기억)할 수도 있으므로 같은 주소로 GET 요청을 할 때 서버에서 가져오는 것이 아닌 캐시에서 가져 올 수 있다.(성능 향상)

- POST : **서버에 자원을 새로 등록** 할 때 사용. request의 본문에는 새로 등록할 데이터를 넣어서 보낸다.

- PUT : **서버의 자원을 request에 들어 있는 자원으로 치환** 할 때 사용. request의 본문에 치환할 데이터를 넣어보낸다.

- PATCH : **서버의 자원의 일부만 수정하고자 할 때** 사용. request의 본문에 데이터를 넣지 않는다.

- OPTIONS : **request 하기 전에 통신 옵션을 설명하기 위해 사용**

위 메서드로 표현하기 애매한 로그인 같은 동작이 있다면 그냥 POST 사용.

주소 하나가 request 메서드를 여러개 가질 수 있다. 

```js
const http = require('http');
const fs = require('fs').promises;

// 데이터 베이스 대용으로 users 객체 선언하여 사용자 정보 저장.
const users = {}; 

http.createServer(async (req, res) => {
  try {
    //  req.method로 HTTP 요청 메서드 구분.
    // 만약 method가 GET이면
    if (req.method === 'GET') {
      // req.url로 요청 주소를 구분. 
      // 만약주소가 / 일때
      if (req.url === '/') {
        // restFront.html 제공
        const data = await fs.readFile('./restFront.html');
        res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
        // res.end 앞에 return을 붙이는 이유 : 
        // res.end를 호출 한다고 해서 함수가 종료되지 않는다. 자바 스크립트에서는 return을 붙이지 않는 한 함수 가 종료되지 않는다. 
        // 그러므로 반드시 return을 붙여야한다.
        return res.end(data);
      } 
        // 주소가 about이면 about.html 파일 제공
        else if (req.url === '/about') {
        const data = await fs.readFile('./about.html');
        res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
        return res.end(data);
      } else if (req.url === '/users') {
        res.writeHead(200, { 'Content-Type': 'application/json; charset=utf-8' });
        return res.end(JSON.stringify(users));
      }
      // /도 /about도 /users도 아니면
      try {
        const data = await fs.readFile(`.${req.url}`);
        return res.end(data);
      } catch (err) {
        // 주소에 해당하는 라우트를 못 찾았다는 404 Not Found error 발생
      }
    } 
      // POST /user 요청에서 사용자를 새로 저장
      else if (req.method === 'POST') {
      if (req.url === '/user') {
        let body = '';
        // 요청의 body를 stream 형식으로 받음
        req.on('data', (data) => {
          body += data;
        });
        // 요청의 body를 다 받은 후 실행됨
        return req.on('end', () => {
          console.log('POST 본문(Body):', body);
          const { name } = JSON.parse(body);
          const id = Date.now();
          users[id] = name;
          res.writeHead(201, { 'Content-Type': 'text/plain; charset=utf-8' });
          res.end('ok');
        });
      }
    } 
      // PUT /user/아이디 요청 : 해당 아이디의 사용자 데이터 수정.
      else if (req.method === 'PUT') {
      if (req.url.startsWith('/user/')) {
        const key = req.url.split('/')[2];
        let body = '';
        req.on('data', (data) => {
          body += data;
        });
        return req.on('end', () => {
          console.log('PUT 본문(Body):', body);
          // 받은 데이터는 문자열이므로 JSON으로 만드는 JSON.parse 과정 필요
          users[key] = JSON.parse(body).name;
          res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
          return res.end('ok');
        });
      }
    } 
      // DELETE /user/아이디 요청 : 해당 아이디의 사용자 제거
      else if (req.method === 'DELETE') {
      if (req.url.startsWith('/user/')) {
        const key = req.url.split('/')[2];
        delete users[key];
        res.writeHead(200, { 'Content-Type': 'text/plain; charset=utf-8' });
        return res.end('ok');
      }
    }
    // 만약 존재하지 않는 파일을 요청했거나 GET 메서드 요청이 아닌 경우 404 NOT FOUND 에러가 응답으로 전송
    res.writeHead(404);
    return res.end('NOT FOUND');
  } catch (err) {
    console.error(err);
    res.writeHead(500, { 'Content-Type': 'text/plain; charset=utf-8' });
    res.end(err.message);
  }
})
  .listen(8082, () => {
    console.log('8082번 포트에서 서버 대기 중입니다');
  });
```