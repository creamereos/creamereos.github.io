---  
layout: post
title: ES6 - Arrow Function / Template Literals
categories: dev
tags: js
comments: true
---

##### Arrow Function 

일반 함수

```js
function sayHello(name) {
    // return이 반드시 들어가야한다.
    return "Hello " + name
}
console.log(sayHello("creamer"));
```

화살표 함수 : 위 코드를 Arrow Function으로 변경 변경 

```js
const sayHello = (name2) => "Hello " + name2; 
// 화살표(=>)가 return을 함축

console.log(sayHello("creamer"));
```

ArrowFunction 함수를 사용하여 이벤트 추가하기.

```html
...
<body>
    <button>Click Me</button>
    <script src="ArrowFunctions.js"></script>
</body>
...
```

```js
const button = document.querySelector('button');

// ArrowFunction의 유일한 규칙 : Argument가 하나라면 괄호()가 필요 없고 2개 이상이면 괄호()가 필요
button.addEventListener("click", event => console.log(event));

button.addEventListener("click", (event, something) => console.log(event));
```

##### TemplateLiterals

```js
const sayHello = (name = "Human") => "Hello " + name;
```

- Variable을 중괄호{}안에 담기

- return 값을 backtick`으로 감싸줘야 작동한다. (Dobule Quote"", Single Quote''로 감싸면 작동하지 않음)

```js 
const sayHello2 = (name2 = "Human") => `Hello ${name2}`;
```

