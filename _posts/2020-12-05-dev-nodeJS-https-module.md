---  
layout: post
title: nodeJS - 요청과 응답
categories: dev
tags: nodeJS
comments: true
---

서버는 클라이언트가 있기에 동작한다. 클라이언트에서 서버로 요청(request)을 보내고, 서버에서는 요청의 내용을 읽고 처리한 뒤 클라이언트에 응답(response)한다. 따라서 **서버**는 **요청을 받는 부분**과 **응답을 보내는 부분**이 있어야한다.

요청과 응답은 **이벤트 방식**이라고 생각하면 된다. **클라이언트로부터 요청이 왔을 때 어떤 작업을 수행**할지 **이벤트 리스너로 미리 등록**해두어야한다.

- 이벤트 리스너를 가진 노드 서버 만들기 (응답 보내기 및 서버 연결)

createServer.js

```js
const http = require('http');

http.createServer((req, res) => {
    // res 객체에는 .writeHead, .write, .end 메서드()가 있다.
    // Header 부분
    // .writeHead : 응답에 대한 정보를 기록하는 메서드
    // 첫번째 인수 - 200 : 성공적인 요청임을 의미
    // 두번째 인수 - 'Content-Type': 'text/html : 컨텐츠 타입이 HTML, charset=utf-8 : 한글 표시를 위해
    res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
    // Body(본문)
    // 첫번째 인수 : 클라이언트로 보낼 데이터. (지금은 HTML 형식을 보냈지만 버퍼로도 보낼 수 있다.) 또한 여러번 호출해서 데이터를 여러개 보내도 된다.
    res.write('<h1>Hello NODE!</h1>');
    // .end : 응답을 종료하는 메서드. 만약 인수가 있다면 그 데이터도 클라이언트로 보내고 응답을 종료한다.
    res.end('<p>Hello Server!</p>');
})  
    // 서버 연결
    // createServer() 메서드 뒤에 .listen 메서드를 붙이고 클라이언트에 공개할 포트번호와 포트 연결 완료 후 실행될 콜백함수를 넣는다. 이제 이 파일을 실행하면 서버는 8080 포트에서 요청이 오기를 기다린다.
    .listen(8080, () => {
        console.log('8080번 포트에서 서버 대기 중!')
    })
 // 8080번 포트에서 서버 대기 중!
```

위 코드를 실행하고 콘솔이 정상 작동하면 웹 브라우저에서 로컬 호스트 주소(http://localhost:8080) 또는 IP주소(http://127.0.0.1:8080)에 접속한다.

- **로컬 호스트** : 현재 컴퓨터으 내부주소(외부에서 접근 할 수 없고 자신의 컴퓨터에서만 접근 가능). 서버 개발 시 테스트 용으로 사용.

- **포트** :서버 내에서 프로세스를 구분하는 번호. 서버는 다양한 작업을 하므로 프로세스에 포트를 다르게 할당하여 들어오는 요청을 구분한다. 포트 번호는 IP 주소 뒤에 콜론(:)과 함께 붙여 사용. (http://127.0.0.1:80)
포트 ex) 21(FTP), 443(HTTPS), 3306(MySQL), 80(HTTP) / 80(HTTP)와 443(HTTPS) 포트는 주소에서 포트 생략 가능

맥에서 1024번 이하의 포트에 연결 할 때 관리자 권한(sudo) 필요. ex) $ sudo node 실행할 파일명

다른 서비스가 사용하고 있는 포트를 사용할 경우 에러 발생(Error: listen EADDRINUSE :::포트 번호) 이런 경우 노드의 포트를 다른 번호로 바꾸면 된다.

- 서버의 소스 코드 변경 : 서버의 소스코드를 변경할 때 서버가 자동으로 변경 사항을 반영하지 않는다. 서버를 종료했다가 다시 실행해야 변경사항이 반영된다.

```js
const http = require('http');

const server = http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type' : 'text/html; charset=utf-8' });
    res.write('<h1>Hello NODE!</h1>');
    res.end('<h1>Hello Server!</h1>');
});

// 서버 연결
// listen 메서드에 콜백함수를 넣는 대신 서버에 listening 이벤트 리스너를 붙여도 된다.
server.listen(8080);

server.on('listening', () => {
    console.log('8080번 포트에서 서버 대기 중!');
});
// error 이벤트 리스너도 추가
server.on('error', (error) => {
    console.error(error);
});
```

- 한 번에 여러 서버를 실행 가능하다. createServer를 원하는 만큼 호출하면 된다. (실무에서 잘 사용하지 않음)

```js
// localhost:8080 주소로 서버에 접속
const http = require('http');

http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type' : 'text/html; charset=utf-8' });
    res.write('<h1>Hello NODE!</h1>');
    res.end('<h1>Hello Server!</h1>');
})

    // 서버 연결
    .listen(8080, () => {
        console.log('8080번 포트에서 서버 대기 중!');
    });

// localhost:8081 주소로 서버에 접속. (포트 번호가 같으면 EADDRINUSE 에러 발생)
http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type' : 'text/html; charset=utf-8' });
    res.write('<h1>Hello NODE!</h1>');
    res.end('<h1>Hello Server!</h1>');
})

    // 서버 연결
    .listen(8081, () => {
        console.log('8081번 포트에서 서버 대기 중!');
    });
```

**[HTTP 상태 코드]**
- 2XX : 성공을 알리는 상태코드 (200:성공, 201:작성 됨)
- 3XX : redirection(다른 페이지 이동) 특정 주소를 입력했는데 다른 주소의 페이지로 넘어 갈 때 이 코드가 사용. (301:영구 이동, 302:임시이동, 304:수정 되지 않음-요청의 응답으로 캐시를 사용했다는 뜻,)
- 4XX : 요청 오류. (400:잘못된 요청, 401:권한 없음, 404:찾을 수 없음)
- 5XX : 서버 오류. 요청은 제대로 왔지만 서버에 오류가 생겼을 때 발생. (500:내부 서버 오류, 502:불량 게이트웨이, 503:서비스를 사용할 수 없음)

res.write와 res.end에 일일이 HTML 코드를 적는 것은 비효율적이므로 미리 HTML 파일을 만들어 두면 좋다. **HTML 파일을 fs 모듈로 읽어서 전송가능**하다.

```js
const http = require('http');
const fs = require('fs').promises;

http.createServer(async (req, res) => {
    try {
        // fs 모듈로 HTML 파일 읽기. 
        const data = await fs.readFile('./server.html');
        res.writeHead(200, { 'Content-Type' : 'text/html; charset = utf-8' });
        // data 변수에 저장된 버퍼를 그대로 클라이언트에 보내기.
        res.end(data);
      //예기치 못한 에러 발생 시 에러 메시지 응답.
    } catch (err) {
        console.error(err);
        // 에러 메시지는 일반 문자열이므로 text/plain 사용
        res.writeHead(200, { 'Content-Type' : 'text/plain; charset = utf-8' });
        res.end(err.message);
    }
})  
    // 이전 예제에서 8080 포트를 사용했으므로 8081 포트 사용.
    .listen(8081, () => {
        console.log('8081번 포트에서 서버 대기 중!')
    })
```

**주의 사항** : 요청 처리 과정 중 에러가 발생했다고 해서 응답을 보내지 않으면 안된다. 요청이 성공했든 실패했든 응답을 클라이언트로 보내서요청이 마무리되었음을 알려야한다. 응답을 보내지 않는다면 클라이언트는 서버로부터 응답이 오길 계속 기다리다가 일정 시간 후 Timeout(시간 초과)처리한다. **무조건 응답을 보내야한다.** 