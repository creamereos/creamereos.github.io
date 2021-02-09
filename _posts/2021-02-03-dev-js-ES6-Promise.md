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
// then + arguments인 value로 불러오기
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

# Promise가 resolve 되면 then 이후의 코드가 실행
# Promise가 reject 되면 catch 이후 코드가 실행

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

# Promise.all

- 다수의 Promise를 한번에 실행. 즉, Promise.all은 다른 Promise들이 전부 진행 될 때까지 기다렸다가 실행된다.

- 모든 Promise가 전무 Resolve되면 마지막 Promise를 return value로 준다.

```js
const promise1 = new Promise((resolv) => {
    setTimeout(resolv, 3000, "First");
})

const promise2 = new Promise((resolv) => {
    setTimeout(resolv, 1000, "Second");
})

const promise3 = new Promise((resolv) => {
    setTimeout(resolv, 2000, "Third");
})

// Promise.all로 불러오는 promise들은 반드시 iterable에 넣어줘야한다.
// Array와 같이 순회 가능한(iterable) 객체.
const alllPromise = Promise.all([promise1, promise2, promise3]);

alllPromise.then(values => console.log(values));
// Promise {<pending>}
// ["First", "Second", "Third"]
// promise1~3이 각각 언제 끝나는지 상관 없이 입력한 순서대로 출력된다.
```

- Promise.all에 reject가 있는 경우 : 하나의 Promise라도 reject 되면 다른 모든 Promise도 reject

```js
const promise1 = new Promise((resolv) => {
    setTimeout(resolv, 3000, "1");
})

const promise2 = new Promise((resolv, rejec) => {
    setTimeout(rejec, 1000, "에러에러에렁2");
})

const promise3 = new Promise((resolv) => {
    setTimeout(resolv, 2000, "3");
})

const alllPromise = Promise.all([promise1, promise2, promise3]);

// promise2가 아닌 allPromise에 .catch 추가 가능.
// 개별 Promise에 .catch를 넣을 수 있지만 한번에 표시하는 게 가독성도 좋고 코드도 간결해짐
alllPromise.then(result => console.log(result)).catch(err => console.log(err));
// 에러에러에렁2
```

# Promise.race 
- resolv로 나온 value든, catch로 나온 error든 가장 빨리 실행된 것만 실행 (race 경주로 생각하면 쉬움. 자주 사용하지는 않음)

```js
const promise1 = new Promise(resolv => {
    setTimeout(resolv, 3000, "First");
});

const promise2 = new Promise(rejec => {
    setTimeout(rejec, 1000, "에렁");
});

const promise3 = new Promise(resolv => {
    setTimeout(resolv, 2000, "Third");
});

const promiseRace = Promise.race([promise1, promise2, promise3]);

promiseRace.then(v => console.log(v)).catch(e => console.log(e));
// Promise {<pending>}
// 에렁

const promise1 = new Promise(resolv => {
    setTimeout(resolv, 1000, "First");
});

const promise2 = new Promise(rejec => {
    setTimeout(rejec, 3000, "에렁");
});

const promise3 = new Promise(resolv => {
    setTimeout(resolv, 2000, "Third");
});

Promise.race([promise1, promise2, promise3]).then(v => console.log(v)).catch(e => console.log(e));
// First
```

# Pormise Finally

- Promise에서 resolve 또는 reject 값을 실행하고 resolve되던 rejec되던 finally 실행 (보통 API 호출 시 사용)

```js
const promise1 = new Promise((resolv, rejec) => {
    setTimeout(resolv, 1000, "First");
}).then(v => console.log(v)).catch(e => console.log(e)).finally(() => console.log("Finally"));
// First
// Finally

const promise2 = new Promise((resolv, rejec) => {
    setTimeout(rejec, 1000, "에렁");
}).then(v => console.log(v)).catch(e => console.log(e)).finally(() => console.log("Finally"));
// 에렁
// Finally
```

# usecase

- fetch : Promise를 return한다. 뭔가를 가지고 올 때 사용. 보통 직접 Promise를 만들기보단 fetch를 사용하여 다른 사람이 만든 Promise를 사용한다.

```js
fetch("https://google.com").then(response => console.log(response)).catch(e => console.log("에러 발생! : google.com은 fetch 할 수 없따"));
// VM1075:1 GET https://google.com/ net::ERR_FAILED
// google.com은 fetch 할 수 없따
```