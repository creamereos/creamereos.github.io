---  
layout: post  
title: JS 21.03.12
subtitle: Scpoe
categories: dev
tags: js
comments: true  
--- 

### scope 

- scope : 내부에는 영향을 미치지만 외부에는 영향을 줄 수 없다. function 내부에 선언된 변수는 해당 fuction의 내부에서만 사용 가능하다.

```js
let sayHi = 'Hi'
// 전역 변수 선언

// function{}은 local scope
function Hiroo(){
	let name = 'creamer';
	//지역 변수 선언
	return sayHi + name;
}

Hiroo() // 'Hi creamer'
name; // ReferenceError
// local scope 안쪽(위 코드에선 function {} 내부)에 선언된 변수는 밖에서 사용 불가능하다.
```

##### local scope vs global scope

전역 변수 : global scope(최상단의 scope) 내에 선언된 변수. 
지역 변수 : local scope(function내 {}) 내에 선언된 변수.

##### scope 특징

- 중첩 가능 (function 안에 function을 넣을 수 있다.)

- global scope는 최상단의 scope로, 전역 변수(전역 스코프에 선언된 변수)는 어디서든 접근이 가능

- local scope에 선언 된 변수는 function 내에서는 global scope에서 선언된 변수보다 더 높은 우선 순위를 가진다. (전역 변수보다 지역 변수를 받아옴)

```js
let name = "Richard"; // 전역 변수

function showName() {
  // function 내에서만 사용할 변수를 새롭게 설정. 전역 변수 name과 다르다.
  let name = "Jack"; // 지역 변수 
  // showName function 안에서만 접근 가능
  console.log(name);
}

console.log(name); // richard
showName(); // jack
console.log(name); // richard
```

```js
let name = "Richard";

function showName() {
  name = "Jack"; // 전역 변수
  // 선언(let)이 없기 때문에, global scope에 있는 name이라는 변수를 가져온다.
  console.log(name); 
}

console.log(name); // richard
showName(); // jack
console.log(name); // jack
```

##### scope 규칙 1 : function scope vs block scope

- block scope : const / let

```js
// let으로 선언된 i는 block scope(for문 내에서만) 적용
for(let i=0; i<5; i++) {
  console.log(i); // 0 1 2 3 4
}

console.log(i); // Uncaught ReferenceError: i is not defined
```

function scope : var 

```js
// var로 선언된 i는 function scope(function 전체 내에서 / block scope(for문 내)를 벗어나도 사용 가능)
for(var i=0; i<5; i++) {
  console.log(i); // 0 1 2 3 4
}
console.log(i); // 5
```

##### 규칙 2: 선언 키워드 - var vs const/let

- var : function 단위로 자신만의 scope를 가진다. (function scope) - 재사용 문제 발생 (호이스팅)

```js
// var 재선언 가능
var a = 1
var a = 2

// var 재할당 가능
var a = 2
a = 2
```

- const/let : Block 단위로 자신만의 scope를 가진다. (block scope) + 선언 불가능

```js
// let 재선언 불가능
let a = 1
let a = 2 
// Uncaught SyntaxError: Identifier 'a' has already been declared

// let 재할당 가능
let a = 1
a = 2

// const 재선언 불가능 
const a = 1
const a = 2
// Uncaught SyntaxError: Identifier 'a' has already been declared

// const 재할당 불가능
const a =1
a = 2
// Uncaught TypeError: Assignment to constant variable.
```

###### 규칙 3: 전역 변수와 window 객체

- 키워드 var로 변수를 선언하면 window 객체와 연결

- **전역 범위에 너무 많은 변수를 선언하지 않도록 주의!** 전역 범위에 전역 변수를 선언하면 다른 파일과 코드들에 영향을 줄 수 있기 때문

**TIP** : 전역 변수를 선언해야 된다면 {} 를 추가로 만들어서 그 안에 설정하자. {} 안에서만 작동되기 때문에 다른 코드에 영향을 주지 않는다.

##### 규칙 4: 선언 없이 초기화 된 전역 변수

- 절대로 선언 키워드(let, const) 없이 변수를 초기화하지 마라! 선언 키워드 없이 변수를 초기화하면 {}안에 있다 하더라도 window 객체에 적용되고 다른 코드들에게 영향을 줄 수 있다.

이러한 실수를 방지하고 싶다면?
**TIP** : Strict Mode 사용 - JS 파일 상단에 'use strict'; 입력

- ex code

```js
let x = 30; // 전역 변수
function get () {
    return x; // 전역 변수 참조
}
let result = get(20);

console.log(result) 
// 30
```

```js
let x = 30; // 전역 변수
function get (x) {
    return x; // 파라미터/인자 참조
}
let result = get(20);

console.log(result) 
// 20
```

```js
let x = 30; // 전역 변수 x
function get () {
     return x; // 전역 변수 참조 
}
function set (value) { 
    let x = value; // x = 10 (무관)
}
set(10);
let result = get(20);

console.log(result) 
// 30
```

```js
let x = 30;
function get () { 
    return x; // 10으로 값이 변한 후의 전역 변수 x 참조
} 
function set (value) { 
    x = value; 
    // 전역 변수 x 참조. 전역 변수 x의 값 10으로 변경
}
set(10); 
let result = get(20);

console.log(result) 
// 10
```

```js
let x = 30; // 전역 변수
function get (x) { return x; } // 인자 20 return
function set (value) { x = value; } // 전역 변수 참조. 전역 변수 x의 값 10으로 변경
set(10);
let result = get(20);
// 20
```

```js
let x = 10; // 전역 변수
function add (y) { 
    return x + y; // x은 전역 변수 참조. y는 strangeAdd의 인자 참조
}

function strangeAdd (x) { 
    return add(x) + add(x);
}

let result = strangeAdd(5);
// 30
```

```js
let x = 10; //전역 변수
function outer () { 
    let x = 20; // 지역 변수
    function inner () {
        return x; //x는 지역변수 참조. 지역 변수가 우선순위가 더 높다.
    }
    return inner(); 
}
let result = outer();
// 20
```

```js
let x = 10; // 전역 변수
function outer () { 
    let x = 20; // 지역 변수
    function inner () { 
        x = x + 10; // 지역 변수 참조
            return x;
    }
    inner(); 
}

outer(); 
let result = x; // 전역 변수 참조
// 10
```

```js
let x = 10; // 전역 변수
function outer() {
    x = 20; // 전역 변수 참조. 전역 변수 값 20으로 변경 
    function inner() {
        let x
        x = x + 20;
        return x; 
    }
    inner();
}
outer(); 
let result = x; // 전역 변수 참조
// 20
```

```js
let x = 10; // 전역 변수
function outer() {
    x = 20; // 전역 변수 참조. 전역 변수 값 20으로 변경
    function inner() {
        x = x + 20; // 20으로 변경 된 전역 변수 참조. 
    }
    inner();
}
outer();
let result = x;
// 40
```