---  
layout: post
title: ES6 - string
categories: dev
tags: js
comments: true
---

# template literals ``backtick

### expression in string

- `backtick` 사이에 넣으면 모든 문자가 string이 된다.

```js
console.log("hello" + " World");
// hello World 

console.log(`"hello" + " World"`);
// "hello" + " World" 

console.log(`hello World`);
// hello World 
```

- `backtick` 사이에서 변수도 실행 가능

```js
const A = "World";

console.log(`Hello ${A}`);
// Hello World
```

- `backtick` 사이에서 함수도 실행 가능

```js
const add = (a, b) => a + b;

console.log(`5 더하기 2는 ${add(5, 2)}이다.`)
// 5 더하기 2는 7이다. 
```

### HTML Fragments

- (ES6 이전) DOM을 사용하여 JS를 사용하여 HTML 설정

```js
const addWelcome = () => {
  const div = document.createElement("div");
  const bigWelcome = document.createElement("h1");
  const smallWelcome = document.createElement("h5");
  bigWelcome.innerText = "Welcome";
  bigWelcome.className = "big"
  smallWelcome.innerText = "Welcome";
  smallWelcome.className = "small"
  div.append(bigWelcome);
  div.append(smallWelcome);
};
```

- (E6 이후) `backtick`을 사용하여 HTML 구조를 만들 수 있음. (스페이스 바 인식)

```js
const addWelcome = () => {
    const div = `
        <div>
            <h1 class="big">Welcome</h1>
            <h5 class="small">Welcome</h5>
        </div>
    `;
}
```

```js
const wrapper = document.querySelector("#app");

const dev = ["Front-end", "Block Chain", "EOSIO"];

const list = `
  <h1>Dev Study</h1>
  <ul> 
    ${dev.map(dev => `<li>${dev}</li>`).join("")}
  </ul>
`;

wrapper.innerHTML = list;
```

- array.map(callback function) : array의 각 element에 callback function을 실행 후 새로운 array 생성

- array.join("separator") : array의 모든 element를 "seperator"로 나눈 후 연결해 하나의 string으로 만든다.

```js
const dev = ["Front-end", "Block Chain", "EOSIO"];
const devList = dev.map(dev => `<li>${dev}</li>`);
console.log(devList);
// ["<li>Front-end</li>", "<li>Block Chain</li>", "<li>EOSIO</li>"]

const sepDevList = devList.join(" ");
console.log(sepDevList);
// <li>Front-end</li> <li>Block Chain</li> <li>EOSIO</li> 
```

- includes() : string에 포함 되어있는지 확인 true 또는 false로 반환

```js
const isEmail = email => email.includes("@");

const checkEmail1 = isEmail("creamer@gmail.com");
console.log(checkEmail1);
// true

const checkEmail2 = isEmail("creamernaver.com");
console.log(checkEmail2);
// false
```

- repeat() : 반복

```js
const ID = "990101-2";
const displayID = `${ID}${"*".repeat(6)}`;

console.log(displayID);
// 990101-2****** 
```

- startsWith(), endsWith() : 시작과 끝 유효성(true / false) 검사

```js
const email = "creamer@gmail.co.kr";
const checkEmail = email.endsWith(".com");
console.log(checkEmail);
// false
```

```js
const email = "creamer@gmail.com";
const checkEmail = email.endsWith(".com");
console.log(checkEmail);
// true
```


