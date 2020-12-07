---  
layout: post
title: nodeJS - 미들웨어 특성 활용
categories: dev
tags: node
comments: true
---

미들웨어는 req, res, next를 파라미터로 가지는 함수(에러 처리 미들웨어만 예외적으로 err, req, res, next)이다.app.use()나 app.get(), app.post() 등으로 미들웨어를 장착한다. 

- 일반적인 미들웨어 함수 문법

```js
app.use((req, res, next) => {
    console.log('모든 요청 실행');
    next();
})
```

- 에러처리 미들웨어

```js
app.use((err, req, res, next) => {
    console.error(err);
    res.status(500).send(err.mesaage);
});
```

- 특정한 주소의 요청에만 미들웨어가 실행되게 하려면 첫 번째 인수로 주소를 넣으면 된다.

```js
app.use('/주소', (req, res, next) => {
    console.log('/주소로 시작하는 요청에서만 실행');
    next();
});
```

- 다수의 미들웨어 장착 : 

```js
app.use (
    morgan('dev');
    express.static('/', path.join(__dirname, 'public')),
    express.json(),
    express.urlencoded({ extended: false}),
    cookieParser(process.env.COOKIE_SECRET),
);
```

다음 미들웨어로 넘어가려면 next 함수를 호출해야한다. 위 미들웨어는 내부적으로 next를 호출하고 있으므로 연달아 쓸 수 있다. 

미들웨어 장착 순서에 따라 어떤 미들웨어는 실행되지 않을 수 있다. next를 호출하지 않는 미들웨어는 res.send나 res.sendFile 등의 메서드로 응답을 보내야한다. express.static과 같은 미들웨어는 정적 파일을 제공할 때 next 대신 res.sendFile 메서드로 응답을 보낸다. 따라서 정적 파일을 제공하는 경우 express.json, express.urlencoded, cookieParser 미들웨어는 실행되지 않는다. 만약 next도 호출하지않고 응답도 보내지 않으면 클라이언트는 응답을 받지 못해 하염없이 기다리게 된다.

- next 함수에 인수를 넣을 수도 있다. 단, 인수를 넣는다면 특수한 동작을 한다.

```js
next(); // 다음 미들웨어로
next(route); // 다음 라우터로

// 그 외의 인수를 넣으면 바로 에러처리 미들웨어로 이동
// 이 때의 인수는 에러 처리 미들웨어의 err 파라미터.
(err, req, res, next) => {}
// 라우터에 에러가 발생할 때 에러를 next(err)을 통해 에러처리 미들웨어로 넘긴다.
next(err);
```

- 미들웨어 간 데이터 전달 방법 

```js
app.use((req, res, next) => {
    // 현재 요청이 처리되는 동안 req.data를 통해 미들웨어 간에 데이터 공유 가능
    // 새로운 요청이 오면 req.data는 초기화.
    req.data = '데이터 넣기';
    // 속성명이 꼭 data일 필요는 없지만 다른 미들웨어와 겹치지 않게 주의.
    // 예를 들어 속성명을 body로 하면 body-parser 미들웨어의 req.body와 겹치기 때문.
    next();
}, (req, res, next) => {
    console.log(req.data); // 데이터 받기
    next();
})
```

- 미들웨어 안에 미들웨어 넣기 : 기존 미들웨어의 기능을 확장 가능

```js
app.use(morgan('dev'));

// 또는

app.use((req, res, next) => {
    morgan('dev')(req, res, next);
});
```

- 미들웨어 안에 조건문 미들웨어 넣기

```js
app.use((req, res, next) => {
    if (process.env.NODE_ENV === 'porduction') {
        morgan('combined')(req, res, next);
    } else {
        morgan('dev')(req,res,next);
    }
})
```