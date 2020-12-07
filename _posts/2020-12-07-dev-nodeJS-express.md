---  
layout: post
title: nodeJS - express
categories: dev
tags: node
comments: true
---

express : 서버를 제작하는 과정에서 불편함을 해소하고 편의 기능을 추가한 웹 서버 프레임워크.


- 우선 package.json을 만들어야한다.

```
$ npm init
```

package.json

```json
{
  "name": "learn-express",
  "version": "0.0.1",
  "description": "익스프레스 공부",
  "main": "app.js",
  "scripts": {
    // nodemon app 커맨드를 통해 app.js를 nodemon으로 실행한다는 뜻   
    "start": "nodemon app"
    // nodemon 모듈로 서버를 자동으로 재시작 할 수 있다.
  },
  "author": "CREAMer",
  "license": "MIT"
}
```
nodemon은 개발용으로만 사용하는 것을 권장한다. 배포 후에는 서버 코드가 빈번하게 변경될 일이 없으므로 nodemone을 사용하지 않아도 된다.

- express, nodemon 설치

```
$ npm i express nodemon
```

app.js

```js
// 모듈 불러오기.
const express = require('express');
// express 내부에 http 모듈이 내장되어있어 서버 역할 가능

// express 모듈을 실행하여 app 변수에 할당
const app = express();

// app.set(키, 값): 데이터 저장
// app.get(키) : 데이터 가져오기

// app.set('port', 포트) : 서버가 실행될 포트 설정
app.set('port', process.env.port || 3000);
// precess.env 객체에 port 속성이 있으면 그 값을 포트 번호로 사용하고
// ||(또는)
// 없다면 3000번 포트를 이용해라

// app.get(주소, 라우터) : 주소에 대한 GET 요청이 올 때 어떤 동작을 할지 적는 부분.
// req(request) : 요청에 관한 정보가 들어있는 객체 파라미터
// res(response) : 응답에 관한 정보가 들어있는 객체 파라미터
app.get('/', (req, res) => {
    res.send('Hello Express');
});
// GET / 요청 시 응답으로 Hello Express를 전송.

// app.get('port')로 포트를 가져와 포트를 연결하고 서버 실행
app.listen(app.get('port'), () => {
    console.log(app.get('port'), '번 포트에서 대기 중');
});
```

```
$ npm start
```

```
[nodemon] 2.0.6
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): *.*
[nodemon] watching extensions: js,mjs,json
[nodemon] starting `node app.js`
3000 번 포트에서 대기 중
```

![](/assets/img/post/2020-12-07-11-39-37.png)

위와 같이 단순한 문자열 대신 HTML로 응답하고 싶다면 res.sendFile 메서드를 사용하면 된다. 단, 파일 의 경로를 path 모듈로 사용해서 지정해야한다.

index.html

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>익스프레스 서버</title>
</head>
<body>
    <h1>익스프레스 서버에 html 파일로 연결 할 수 있다.</h1>
    <p>단순한 문자열 대신 HTML로 응답하고 싶다면 res.sendFile 메서드를 사용하면 된다.<br/>
        단, 파일의 경로를 path 모듈로 사용해서 지정해야한다.        
    </p>
</body>
</html>
```

app.js

```js
const express = require('express');
// path 모듈 추가로 불러오기 : sendFile 메서드를 사용하기 위해
const path = require('path');

const app = express();

app.set('port', process.env.port || 3000);

app.get('/', (req, res) => {
    // 바뀐 부분 : sendFile(path 경로 지정)
    res.sendFile(path.join(__dirname, '/index.html'));
});
app.listen(app.get('port'), () => {
    console.log(app.get('port'), '번 포트에서 대기 중');
});
```

![](/assets/img/post/2020-12-07-12-03-39.png)
