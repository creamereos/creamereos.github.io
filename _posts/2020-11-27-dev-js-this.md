---  
layout: post
title: JS - this
categories: dev
tags: javascript
comments: true
---
this는 고정된 것을 가리키는 것이 아닌 함수 내에서 함수 호출 맥락(context)를 의미한다. 맥락이라는 것은 상황에 따라서 달라진다는 의미인데 즉 함수를 어떻게 호출하느냐에 따라서 this가 가리키는 대상이 달라진다는 뜻이다. 

**함수와 객체**의 관계가 느슨한 자바스크립트에서 **this는 이 둘을 연결**시켜주는 실질적인 연결점의 역할을 한다.

- 객체 안에 없는 함수를 호출 시 this는 전역 객체를 가리킨다. 객체에 속해있지 않는 함수는 전역객체(window)에 속해있기 때문에. 

```javascript
function 함수(){
    if(window === this){
        // this는 전역객체인 window를 가리킨다.
        console.log("window === this");
    }
}
함수(); 

//window === this
// this === 전역객체
```

- object 안에 속해있는 method(object{}에 속해 있는 function)의 this는 object를 가리킨다.

```javascript
const A {
    func : function(){
        if(A === this){
            console.log("A === this");
        }
    }
}

A.func();
// A === this
```

- 생성자(new)의 this : 함수 안에 소속되어있는 this는 소속되어있는 객체를 가리킨다. 

```javascript
const funcThis = null;

// 함수 정의
function Func(){
    funcThis = this;
}

// 일반 함수 호출
const o1 = Func();
if(funcThis === window){
    console.log('window');
}
// window

// 생성자로 호출
const o2 = new Func():
if(funcThis === o2){
    console.log('o2');
}
// o2
//생성자는 빈 객체를 만든다. 그리고 이 객체내에서 this는 만들어진 객체를 가르킨다.
```

- 생성자는 빈 객체를 만든다. 그리고 이 객체내에서 this는 만들어진 객체를 가르킨다. 이는 매우 중요하다. 생성자가 실행되기 전까지는 객체는 변수에도 할당될 수 없기 때문에 this가 아니면 객체에 대한 어떠한 작업을 할 수 없기 때문이다. 

```javascript
function Func(){
    console.log(o);
}

const o = new Func();
//undefined
```

- 함수 리터럴
함수 객체 생성

```javascript
function 함수명(파라미터){실행할 값}

// {} 객체
```

- 객체 리터럴

```javascript
const 변수명 = { };

new Object
```

- 배열 리터럴

```javascript
const 변수명 = [];

new Array()
```

### apply / call

```javascript
const o = {}
const p = {}

function func(){
    switch(this){
        case o:
            console.log('o');
            break;
        case p:
            console.log('p');
            break;
        case window:
            console.log('window');
            break;
    }
}

func(); //window
func.apply(o); // o
func.apply(p); // p
```