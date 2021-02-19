---  
layout: post
title: Node JS - Const / let
subtitle:
categories: dev
tags: js
comments: true  
--- 

# let, const (변수 선언)
이전에는 변수 선언 시 var를 사용했지만 const와 let으로 대체.

### const와 let의 공통점 : block scope(범위)

- var 사용 시 정상 실행. var는 함수 스코프(function scope)를 가지므로 if 문의 블록과 관계 없이 접근 가능.

~~~
if (true) {
    var x= 3;
}
console.log(x);
// 3
~~~

- const 또는 let 사용시 오류 발생. **const와 let은 블록 스코프(block scope)를 가지므로 블록 밖의 변수에 접근 할 수 없음.**

- **블록의 범위는 {} 중괄호 사이.**

- 블록 스코프를 사용하는 이유는 호이스팅 같은 문제를 해결 가능하며, 코드 관리도 수월해서.

~~~
if (true) {
    const y= 3;
}
// const와 let은 블록 밖의 변수 y에 접근 못한다.
console.log(y);

// VM331:4 Uncaught ReferenceError: y is not defined
at <anonymous>:4:13
~~~

### const와 let의 차이점 : 상수(const)와 변수(let)

- const : **한 번 값을 할당하면 다른 값을 할당 불가능. const로 선언한 변수를 상수**라고 부르기도 한다.

~~~
다른 값을 할당하려고하면 에러 발생

const a = 0;
a = 1;
//VM401:2 Uncaught TypeError: Assignment to constant variable. at <anonymous>:2:3
~~~

~~~
또한 초기화 할 때 값을 할당하지않으면 에러 발생.
const c;
// VM539:1 Uncaught SyntaxError: Missing initializer in const declaration.
~~~

- let :

~~~
let b = 0;
b =1;
//1

변수 b에 재할당 가능.
~~~

### const와 let 중 어떤 것을 사용해야할까?

- **const** : **기본적**으로 사용
- **let** : **다른 값을 할당해야 하는 상황**이 생겼을 때 사용
