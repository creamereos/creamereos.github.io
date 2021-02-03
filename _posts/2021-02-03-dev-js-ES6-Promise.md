---  
layout: post
title: ES6 - Promise
categories: dev
tags: js
comments: true
---

- Promise : 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과 값을 나타냄. 순차적으로 처리되는게 아니라 한번에 실행 (동기성 / 비동기성). 

- Promise의 핵심은 아직 모르는 value를 작업 가능하다. (뒤늦게 불러와도 값을 넣어 줄 수 있으므로)

- create Promise

```js
const amICool = new Promise((resolv, rejec) => {});

console.log(amICool);
// Promise {<pending>}
// Promise를 resolve해야 값을 나타내고 Promise를 끝낼 수 있다.
```

```js
const amICool = new Promise((resolv, rejec) => {
    // setTimeout(() => {
    //     resolv("Yes You are!");
    // }, 3000)
    // 위 코드는 아래와 같이 변경 가능
    setTimeout(resolv, 3000, "Yes You are!");
});

console.log(amICool);

setInterval(console.log, 1000, amICool);

// setInterval 때문에 1초마다 실행
// Promise {<pending>} 
// Promise {<pending>}
// Promise {<pending>}

// Promise의 resolve가 3초 뒤에 실행 되므로 4번째 부터는 값이 출력
// Promise {<fulfilled>: "Yes You are!"}
// Promise {<fulfilled>: "Yes You are!"}
// Promise {<fulfilled>: "Yes You are!"}
// ...
// Promise {<fulfilled>: "Yes You are!"}
```

- using promise + Then : promise가 끝난 이후의 명령어를 전달

```js
// then + value로 불러오기
const amICool = new Promise((resolv, rejec) => {
    resolv("Yes you are!");
});

amICool.then(value => console.log(value));
// Yes You are!

// then + function으로 불러오기
const thenFuntion = value => console.log(value);

amICool.then(thenFuntion);
// Yes You are!
```

- reject : promise에 오류가 있는 경우 사용

```js
const amICool = new Promise((resolv, rejec) => {
    setTimeout(rejec, 3000, "nope");
});

amICool.then(value => console.log(value));
// Promise {<pending>}
// Uncaught (in promise) nope
// 에러를 잡지 못해서 위와 같은 오류 코드 나옴
```

- reject를 catch와 함께 사용하면 에러 값을 받을 수 있음.

```js
const amICool = new Promise((resolv, rejec) => {
    setTimeout(rejec, 3000, "nope");
});

amICool
    // Promise가 resolve 되면 then 이후의 코드가 실행
    .then(result => console.log(result))
    // Promise가 reject 되면 catch 이후 코드가 실행
    .catch(error => console.log(error));
// Promise {<pending>}
// nope
```

- chaining promise

```js
const amICool = new Promise((resolv, rejec) => {
    resolv(2);
});

const timesTwo = number => number * 2;

amICool
    .then(timesTwo)
    .then(timesTwo)
    .then(timesTwo)
    .then(timesTwo)
    .then(result => console.log(result));
// 32
```