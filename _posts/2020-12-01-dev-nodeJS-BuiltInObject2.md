---  
layout: post
title: nodeJS - 내장 객체(timer, __filename, __dirname)
categories: dev
tags: nodeJS
comments: true
---

- setTimeout(콜백함수, 밀리초) : 밀리초 **이후** 콜백 함수 **실행** 

- setInterval(콜백함수, 밀리초) : 밀리초 **마다** 콜백 함수 **실행**

- setImmediate(콜백함수) : **즉시 실행**

- 타이머 취소 : clearTimeout(종료할 setTimeout의 아이디), clearInterval(종료할 setInterval의 아이디),  clearImmediate(종료할 setImmediated의 아이디)

- setImmediate(콜백함수)와 setTimeout(콜백함수, 0)의 차이점 : 특수한 경우(I/O작업의 콜백 함수 안에서 타이머를 호출하는 경우), setImmediate(콜백함수)가 먼저 실행. **헷갈리지 않도록 setTimeout(콜백함수, 0)을 사용하지 않는 것을 권장**

- ex code

```javascript
const timeout = setTimeout(() => {
    console.log('1.5초 후에 실행');
}, 1500);

const interval = setInterval(() => {
    console.log('1초마다 실행');
}, 1000);

const timeout2 = setTimeout(() => {
    console.log('실행되지 않습니다.')
}, 3000);

setTimeout(() => {
    clearTimeout(timeout2);
    clearInterval(interval);
}, 2500);

const immediate = setImmediate(() => {
    console.log('즉시 실행');
});

const immediate2 = setImmediate(() => {
    console.log('실행되지 않습니다.');
});

clearImmediate(immediate2);

// 즉시 실행
// 1초 마다 실행
// 1.5초 후에 실행
// 1초 마다 실행
```

###  __filename, __dirname

- filename.js
```javascript
console.log(__filename);
//__filename : 현재 파일명
console.log(__dirname);
//__dirname : 현재 파일 경로
```

- console
```javascript
$ node filename.js
```