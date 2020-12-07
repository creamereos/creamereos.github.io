---  
layout: post
title: nodeJS - 미들웨어
categories: dev
tags: node
comments: true
---

미들웨어는 익스프레스의 핵심이다. request와 response의 중간에 위치하여 미들웨어라고 부른다.(라우터와 에러 핸들러 또한 미들웨어의 일종). 미들웨어는 요청과 응답을 조작하여 기능을 추가하기도하고, 나쁜 요청을 걸러내기도 합니다.

미들웨어는 app.use와 함께 사용된다. ex: app.use(미들웨어)

- 익스프레스 서버에 미들웨어 연결하기

app.js

```js

const express = require('express');
const path = require('path');

const app = express();

app.set('port', process.env.port || 3000);

// [미들웨어]
// 미들웨어는 위에서부터 아래로 순서대로 실행되면서 요청과 응답 사이에 특별한 기능 추가 가능
// app.use에 parameter가 req, res, next인 함수 넣으면 된다.
app.use((req, res, next) => {
    console.log('모든 요청에 실행.');
    // next 파라미터는 다음 미들웨어로 넘어가는 함수. next를 넣지 않으면 다음 미들웨어가 실행되지 않는다.
    next();
});

app.get('/', (req, res, next) => {
    console.log('GET / 요청에서만 실행');
    next();
}, (req, res) => {
    throw new Error('에러는 에러 처리 미들웨어로 간다.');
    // app.get('/')의 두번째 미들웨어에서 에러가 발생하고, 이 에러는 바로 밑에 있는 [에러처리 미들웨어]로 전달 된다.
});

// [에러 처리 미들웨어]
app.use((err, req, res, next) => {
    console.error(err);
    res.status(500).send(err.mesaage);
});

app.listen(app.get('port'), () => {
    console.log(app.get('port'), '번 포트에서 대기 중');
});
```

주소를 첫 번째 인수로 넣어주지 않는다면 미들웨어는 모든 요청에서 실행되고 주소를 넣는다면 해당 요청에만 실행.

- app.use(미들웨어) : 모든 요청에서 미들웨어 실행

```js
app.use((req, res, next) => {
    console.log('모든 요청에 실행.');
    next();
});
```

- app.use('/주소', 미들웨어) : 주소로 시작하는 요청에서 미들웨어 실행

```js
app.use('/주소', (req, res, next) => {
    console.log('/주소로 시작하는 요청에서만 실행');
    next();
});
```

- app.post('/주소', 미들웨어) : 주소로 시작하는 POST 요청에서 미들웨어 실행

```js
app.post('/주소', (req, res, next) => {
    console.log('/주소로 시작하는 POST 요청에서만 실행');
    next();
});
```

- app.get, app.use 등의 라우터에 여러개의 미들웨어 장착 가능.

```js
// app.get 라우터에 2개의 미들웨어가 장착되어있다.
app.get('/', (req, res, next) => {
    console.log('GET / 요청에서만 실행');
    // 이런 경우에도 next를 호출해야 다음 미들웨어로 넘어갈 수 있다.
    next();
}, (req, res) => {
    throw new Error('에러는 에러 처리 미들웨어로 간다.');
});
```

- 에러 처리 미들웨어 : 에러처리 미들웨어의 parameter는 4개(err, req, res, next)다. 모든 parameter를 사용하지 않더라도 반드시 4개여야한다. 첫번째 parameter인 err에는 에러에 관한 정보가 담겨있다. 에러 처리 미들웨어를 연결하지 않아도 기본적으로 익스프레스가 에러를 처리하긴 한다. 하지만 **실무에서는 직접 에러 처리 미들웨어를 연결해주는 것이 좋다.** 에러 처리 미들웨어는 특별한 경우를 제외하고 가장 아래에 위치.

```js
// err에는 에러에 관한 정보가 담겨있다.
app.use((err, req, res, next) => {
    console.error(err);
    // res.status 메서드로 HTTP 상태 코드를 지정할 수 있다. 기본값은 200(성공)
    res.status(500).send(err.mesaage);
});

// 모든 요청에서 실행.
// GET / 요청에서만 실행
// Error: 에러는 에러 처리 미들웨어로 간다.
// ...
```

### 실무에서 자주 사용하는 패키지

```
$ npm i morgan cookie-parser express-session dotenv
```

dotenv(process.env를 관리하기 위한 패키지)를 제외한 다른 패키지는 미들웨어이다.

- .env 파일 생성

.env

```
COOKIE_SECRET=cookiesecret
```

app.js

```js

const express = require('express');
// 설치한 패키지를 불러오기
const morgan = require('morgan');
const cookieParser = require('cookie-parser');
const session = require('express-session');
// .env 파일을 읽어서 process.env로 만든다.
const dotenv = require('dotenv');
const path = require('path');

dotenv.config();
const app = express();
app.set('port', process.env.port || 3000);

// 불러온 패키지를 app.use에 연결.
// 미들웨어 파라미터인 req, res, next는 미들웨어 내부에 있다.
app.use(morgan('dev'));
app.use('/', express.static(path.join(__dirname, 'public')));
app.use(express.json());
app.use(express.urlencoded({ extended: false}));
app.use(cookieParser(process.env.COOKIE_SECRET));
app.use(session({
    resave: false,
    saveUninitialized: false,
    // 보안과 설정의 편의성을 위해 process.env를 별도의 파일로 관리
    // 비밀 키들을 소스 코드에 귿로 적어두면 소스코드가 유출 되었을때 키도 같이 유출되기 때문.
    // 소스코드가 유출되더라도 .env 파일만 잘관리하면 비밀키는 지킬 수 있다.
    secret: process.env.COOKIE_SECRET,
    // process.env.COOKIE_SECRET에 cookiesecret 값이 할당.(COOKIE_SECRET=cookiesecret)
    cookie: {
        httpOnly: true,
        secure: false,
    },
    name: 'session-cookie',
}));

app.use((req, res, next) => {
// ... 생략 ()
```
console

```
3000 번 포트에서 대기 중
모든 요청에서 실행.
GET / 요청에서만 실행
Error: 에러는 에러 처리 미들웨어로 간다.
...
<!-- morgan 미들웨어 부분 -->
GET / 500 11.109 ms - -
```

- morgan : 요청과 응답에 대한 정보를 콘솔에 기록

```js
app.use(morgan('dev));

// GET / 500 11.109 ms - -
// [HTTP 메서드] [주소] [HTTP 상태 코드] [응답 속도]-[응답바이트]

// dev외의 인수로 combined,common,shor,tiny 등을 넣을 수 있다. 
// 개발 환경에서는 dev를, 배포환경에서는 combined를 주로 사용
```

- static : 정적인 파일(html, css, js, img 등)들을 제공하는 라우터 역할을 하는 미들웨어. (기본적으로 제공되기에 따로 설치할 필요 없이 express 객체 안에서 꺼내 사용하면 된다.)

```js
app.use('요청 경로', express.static('실제 경로'));

app.use('/', express.static(path.join(__dirname, 'public')));
```
함수의 인수로 정적 파일이 담겨 있는 폴더(위 코드 예시에서는 public)를 지정하면 된다.
예를 들어 public/stylesheets/style.css는 localhost:3000/stylesheets/style.css로 접근 가능
public 폴더를 만들고 css나 js, 이미지 파일들을 public 폴더에 넣으면 브라우저에서 접근 가능. 단, body-parser 미들웨어는 멀티파트(이미지, 동영상, 파일)데이터는 처리 못함. 멀티파트 데이터를 처리하려면 multer 모듈 사용

실제 경로에는 public이 들어있지만 요청 경로에는 public이 들어있지않다. 서버의 폴더 경로와 요청 경로가 다르므로 외부인이 서버의 구조를 쉽게 파악할 수 없고 이는 보안에 큰 도움이된다.

또한 정적인 파일들을 알아서 제공해주므로 fs.readFile로 파일을 직접 읽어서 전송할 필요가 없다. 만약 요청 경로에 해당하는 파일이 없으면 자동으로 next를 호출한다. (파일을 발견하면 다음 미들웨어는 실행되지 않는다.) 

- body-parser : 요청의 본문에 있는 데이터를 해석하는 req.body 객체 생성. 보통 폼데이터나 AJAX 요청의 데이터를 처리. (body-parser 미들웨어의 일부 기능이 express에 내장 되었으므로 따로 설치할 필요가 없다. 

```js
// JSON 형식의 데이터 전달 방식
app.use(express.json());

// 주소 형식으로 데이터 전송. (폼 전송시 주로 사용)
// { extended: } 옵션이 false면 노드의 querystring 모듈을 사용하여 쿼리스트링 해석
// { extended: } 옵션이 true면 노드의 qs 모듈(npm 패키지)을 사용하여 쿼리스트링 해석
app.use(express.urlencoded({ extended: false}));
```

단, Raw(본문이 버퍼데이터 일때), Text(텍스트 데이터 일때) 형식의 데이터를 해석하려면 직접 설치)

```
$ npm i body-parser
```

```js
const bodyParser = require('body-parser');
app.use(bodyParser.raw());
app.use(bodyParser.text());
```

또한 body-parser 미들웨어는 req.on('data')와 req.on('end') 대신 내부적으로 스트림을 처리해 req.body에 추가.

예를 들어 JSON 형식으로 { name: 'CREAMer, book: 'nodejs }를 본문으로 보낸다면 req.body에 그대로 들어간다.

- cookie-parser : 쿠키를 생성할 때 쓰이는게 아니라 요청에 동봉된 쿠키를 해석해 req.cookies 객체른 만든다. (parseCookies 함수와 기능이 비슷)

```js
// 첫번째 인수로 비밀 키를 넣을 수 있다.
app.use(cookieParser(비밀키));
// 서명된 쿠키가 있는 경우 제공한 비밀 키를 통해 해당 쿠키가 내 서버가 만든 쿠키임을 검증 가능
```

예를 들어 name=creamer 쿠키를 보냈다면 req.cookies는 { name: 'creamer'}가 된다. 유효기간이 지난 쿠키는 알아서 걸러낸다.

쿠키는 클라이언트에서 위조하기 쉬우므로 비밀키를 통해 만들어낸 서명을 쿠키 값 뒤에 붙인다. 서명이 붙으면 name=creamer.sign과 같은 형식이 된다. 서명된 쿠키에는 req.cookies 대신 req.signedCookies 객체에 들어 있다.

- res.cookie(키, 값, 옵션)/res.clearCookie(키, 값, 옵션) 쿠키 생성/제거
옵션으로는 domain, expires, httpOnly, maxAge, patch, secure, sigend 등이 들어갈 수 있다.

옵션 값 signed : true를 설정하면 쿠키 뒤에 서명이 붙는다. 내 서버가 쿠키를 만들었다는 것을 검증 할 수 있으므로 대부분 서명 옵션을 켜두는 것이 좋다. 서명을 위한 비밀키는 cookieParser 미들웨어에 인수로 넣은 process.env.COOOKIE_SECRET이 된다.

```js
// 쿠키 생성
res.cookie('name', 'creamer', {
    expires: new Date(Date.noew() + 900000),
    httpOnly: true,
    secure: true,
});

// 쿠키 제거
res.clearCookie('name', 'creamer', { httpOnly: true, secure: true});
// 쿠키 제거시 키 값 외 옵션도 정확히 일치해야 쿠키가 지워진다.
// 단, expires나 maxAge 옵션은 일치할 필요가 없다.
```

- express-session : 세션 관리용 미들웨어. 로그인 등의 이유로 세션을 구현하거나 특정 사용자를 위한 데이터를 임시적으로 저장해 둘 때 유용. 세션은 req.session 객체 안에 유지된다.

```js
// express-session은 인수로 세션에 대한 설정을 받는다.
app.use(session({
    // reseve : 요청이 올 때 세션에 수정 사항이 생기지 않더라도 세션을 다시 저장 할지 설정
    resave: false;
    // saveUninitialized : 저장할 내역이 없더라도 처음부터 세션을 생성할지 설정
    saveUninitialized: false;
    // 안전하게 쿠키를 클라이언트에 전송하기 위해 쿠키에 서명 추가.
    // cookie-parser의 secret과 같게 설정하는 것이 좋다.
    secret: procee.env.COOKIE_SECRET,
    // [cookie 옵션 설정]
    // maxAge, domain, path, expires, sameSite, httpOnly, secure 등 일반적인 쿠키 옵션 모두 제공
    cookie: {
        // httpOnly가 ture면 클라이언트에서 쿠키를 확인하지 못함
        httpOnly: ture,
        // secure가 false면 https가 아닌 환경에서도 사용 가능
        secure: false,
        // 실제 배포시에는 https를 적용하고 secure도 true로 설정하는 것이 좋다.
    },
    // 세션 쿠키의 이름 설정 (기본 이름 : connect.sid)
    name: 'session-cookie',
}));
```

- 세션 등록 / 아이디 확인 / 제거

```js
req.session.name = 'creamer'; // 세션 등록
req.sessionID; // 세션 아이디 확인
req.session.destroy(); // 세션 모두 제거
```

express-session에서 서명한 쿠키 앞에는 s: 가 붙는다. 실제로는 encodeURIComponet 함수가 실행되어 s%3A가 된다. s%3A 뒷부분이 실제 암호화 된 쿠키 내용이다. s%3A가 붙은 경우, 이 쿠키가 express-session 미들웨어에 의해 암호화된 것이라고 생각하면된다.




