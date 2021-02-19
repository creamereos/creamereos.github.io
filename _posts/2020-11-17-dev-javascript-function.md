---  
layout: post
title: JS - function basic
subtitle:
categories: dev
tags: js
comments: true
---

- function : 하나의 logic을 **재실행** 할 수 있도록 하여 코드의 **재사용성,유지보수 용이, 가독성**을 높여준다. 

```javascript
function 함수명( [인자...[,인자]] ){
   코드
   return 반환값
}

- 함수 호출(재실행)
함수명( [인자...[,인자]] 
```

```javascript
function A(){
    i = 0;
    while(i < 10){
        document.write(i);
        i += 1;
}

- 함수 호출(재실행)
A();

괄호();를 넣음으로써 변수와 구분  
```

- return(출력:outputs)

```javascript
function 함수명() {
    return '출력';
}
```

- argument:변수 인자 (입력:input)
argument는 function으로 유입되는 input 값을 의미하는데, 어떤 값을 argument로 전달하느냐에 따라서 함수가 return하는 값이나 method의 동작방법을 다르게 할 수 있다.

```javascript
function 함수명(매개변수=parameter) {
    return(출력) 인자;
}

function_name(인자=argument);

function_name(1); // 1
function_name('arg'); // arg
```

- 다수의 argument

```javascript
function 함수명(매개변수1=parameter1, 매개변수2=parameter2) {
    return(출력) 매개변수+매개변수;
}


function_name(인자1=argument1, 인자2=argument2);

function_name(1, 5); // 6
function_name('CREAM', 'er'); // CERAMer
```

- 함수 정의 방법2

```javascript
const numbering = function (){
    i = 0;
    while(i < 10){
        document.write(i);
        i += 1;
    }   
}
numbering();

### 익명 함수

```
- 익명 함수 정의 : 1회성 호출

```javascript
(function (){
    i = 0;
    while(i < 10){
        document.write(i);
        i += 1;
    }   
})();
```