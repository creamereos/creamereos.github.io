---  
layout: post
title: ES6 - variables
categories: dev
tags: js
comments: true
---

### const & let

 - const : variable 재정의 불가 (defualt)

 ```js
const name = "creamer";

name = "Terry";
// TypeError: "name" is read-only

console.log(name);
// creamer
 ```

- let : variabel 재정의 가능 (ES6 이전의 var과 같은 역할. var 사용 금지)

```js
let name = "creamer";

name = "Terry";

console.log(name);
// Terry
```

### block scope {}

- const와 let이 {} 안에서 사용 되었다면 적용 범위는 block scope{} 안

```js
if(true) {
    const name = "creamer";
    let age = 30;
}

console.log(name);
// Uncaught ReferenceError: name is not defined
console.log(age);
// Uncaught ReferenceError: age is not defined
```

- const와 let이 {} 밖에서 사용 되었다면 적용 범위는 무관

```js
const name = "creamer";
let age = 30;

if(true) {
    console.log(name);
    console.log(age);
}
// creamer
// 30
```



