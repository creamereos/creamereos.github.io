---  
layout: post
title: nodeJS - 스레드 풀
categories: dev
tags: nodeJS
comments: true
---

비동기 메서드 사용시 fs 메서드를 여러번 실행해도 백그라운드에서 동시에 처리되는데, 바로 스레드풀 덕분이다. fs외에도 내부적으로 스레드 풀을 사용하는 모듈로는 crypto, zlib, dns, lookup등이 있다.

- 스레드풀 확인하기

threadpool.js

```js
const crypto = require('crypto')

const pass = 'pass';
const salt = 'salt';
const start = Date.now();

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
   console.log('1:', Date.now() - start); 
});

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
    console.log('2:', Date.now() - start); 
 });

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
    console.log('3:', Date.now() - start); 
 });

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
    console.log('4:', Date.now() - start); 
 });

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
    console.log('5:', Date.now() - start); 
 });

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
    console.log('6:', Date.now() - start); 
 });

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
    console.log('7:', Date.now() - start); 
 });

crypto.pbkdf2(pass, salt, 1000000, 128, 'sha512', () => {
    console.log('8:', Date.now() - start); 
 });

// 첫 실행
// 3: 4748
// 1: 4808
// 4: 4950
// 2: 5021
// 5: 8940
// 6: 9016
// 7: 9049
// 8: 9089

// 두번째 실행
// 2: 4196
// 1: 4214
// 3: 4226
// 4: 4337
// 5: 8919
// 6: 8924
// 7: 8957
// 8: 9031
```

위 코드를 실행할 떄마다 시간과 순서가 달라진다. 스레드 풀이 작업을 동시에 처리하므로 8개의 작업 중에서 어느것이 먼저 처리 될지 모른다.
하지만 하나의 규칙을 발견 할 수 있따. 1~4와 5~8이 그룹으로 묶여져 있고, 5~8이 1~4보다 시간이 더 소요된다.
기본적인 스레드 풀의 개수가 4개이기 때문이다. 스레드풀이 4개이므로 처음 4개의 작업이 동시에 실행되고 그것들이 종료되면 다음 4개의 작업이 실행된다.
(만약 컴퓨터 코어 개수가 4보다 작다면 다른 결과가 나옴)

스레드풀을 직접 컨트롤 할 수 없지만 개수를 조절 할 수 있다. 

```js
// 터미널에 THREADPOOL_SIZE=1을 입력 후 다시 실행

$ UV_THREADPOOL_SIZE=1
// = process.env.UV_THREADPOOL_SIZE = 1

$ node threadpool
```

위 코드를 터미널 콘솔에서 실행하면 작업이 순서대로 진행된다. 스레드 풀의 개수를 하나로 제한하여 작업이 한번에 하나씩 처리 되었기 때문이다. 스레드의 개수를 크게 할 때는 자신의 컴퓨터 코어 개수와 같거나 많게 두어야 뚜렷한 효과가 발생한다.
