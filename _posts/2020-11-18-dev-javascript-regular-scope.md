---  
layout: post
title: JS - scope
categories: dev
tags: js
comments: true
---

유효범위(Scope)는 변수의 수명을 의미한다.


### 전역 변수, 지역 변수
**함수 밖**에서 변수를 선언하면 그 변수는 **전역변수**가 된다. 전역변수는 에플리케이션 전역에서 접근이 가능한 변수다. 다시 말해서 어떤 함수 안에서도 그 변수에 접근 할 수 있다.

**지역변수**의 유효범위는 **함수 안**이고, 전역변수의 유효범위는 에플리케이션 전역인데, **같은 이름의 지역변수와 전역변수가 동시에 정의**되어 있다면 **지역변수가 우선**

**변수를 선언할 때는 꼭 let을 붙이는 것을 습관화해야 한다. 전역변수를 사용해야 하는 경우라면 그것을 사용하는 이유를 명확히 알고 있을 때 사용하도록 하자!**

```javascript
let 변수(전역) = '1';
function 함수명 (){
    alert(변수);
}
fscope();
//1

let 변수(전역) = '1';
function 함수명(){
    let 변수(지역) = '2' 
    alert(변수);
}
함수명();
//2
```

### 전역 변수를 사용 하는 경우
불가피하게 전역변수를 사용해야 하는 경우는 하나의 객체를 전역변수로 만들고 객체의 속성으로 변수를 관리하는 방법을 사용한다.

```javascript
const MYAPP = {}
// 전역 변수 = {}객체

// 객체의 속성으로 변수 관리
MYAPP.calculator = {
    'left' : null,
    'right' : null
}
MYAPP.coordinate = {
    'left' : null,
    'right' : null
}
 
MYAPP.calculator.left = 10;
MYAPP.calculator.right = 20;
function sum(){
    return MYAPP.calculator.left + MYAPP.calculator.right;
}
document.write(sum());
```
전역변수를 사용하고 싶지 않다면 아래와 같이 **익명함수**를 호출함으로서 이러한 목적을 달성할 수 있다. 이는 자바스크립트에서 로직을 모듈화하는 일반적인 방법

```javascript
(function(){
    var MYAPP = {}
    // function 안에 들어오므로 지역변수
    MYAPP.calculator = {
        'left' : null,
        'right' : null
    }
    MYAPP.coordinate = {
        'left' : null,
        'right' : null
    }
    MYAPP.calculator.left = 10;
    MYAPP.calculator.right = 20;
    function sum(){
        return MYAPP.calculator.left + MYAPP.calculator.right;
    }
    document.write(sum());
}())
// ()를 function 뒤에 붙이고 ()로 function을 감싸주면 익명함수로 만들어 함수를 바로 실행해준다.
```

### scope의 대상
자바스크립트는 **함수(보다 넓은)에 대한 유효범위 제공**. 다른 많은 언어들은 블록(대체로 {,})에 대한 유효범만을 제공

```javascript
for(var i = 0; i < 1; i++){
    var name = 'CREAMer';
}
alert(name);
//CREAMer

```
자바에서는 아래의 코드는 허용되지 않는다. name은 지역변수로 for 문 안에서 선언 되었는데 이를 for문 밖에서 호출하고 있기 때문이다.

```java
for(int i = 0; i < 10; i++){
    String name = "CREAMer";
}
System.out.println(name);
//SyntaxError: Unexpected identifier
```

### 정적 유효범위
자바스크립트는 함수가 선언된 시점에서의 유효범위를 갖는다. 이러한 유효범위의 방식을 정적 유효범위(static scoping), 혹은 렉시컬(lexical scoping)이라고 한다. 

```javascript
let i = 5;
 
function a(){
    let i = 10;
    b();
}
 
function b(){
    document.write(i);
}
 
a();
//5
```

```javascript
```

```javascript
```