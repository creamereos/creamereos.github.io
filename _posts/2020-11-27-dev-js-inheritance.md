---  
layout: post
title: JS - 상속(inheritance)
categories: dev
tags: js
comments: true
---
**상속**은 **객체의 로직(메소드,변수)을 그대로 물려 받는 또 다른 객체를 만들 수 있는 기능**을 의미한다. 또한 **기존의 로직을 수정하고 변경**해서 파생된 **새로운 객체**를 만들 수 있게 해준다. 이처럼 부모의 기능을 계승 발전할 수 있는 것이 **상속의 가치**다.


```javascript
// object(객체) 생성. 생성자 함수는 일반함수와 구분하기 위해서 첫글자를 대문자로 표시
function Person(name){
    this.name = name;
}
// property(객체의 변수) 지정
Person.prototype.name = null;
// method(객체의 함수)
Person.prototype.introduce = function(){
    return 'My name is '+ this.name;
}

// 부모(Person) 객체를 상속 받을 object(객체) 생성.
function Programmer(name){
    this.name = name;
}

// 상속. Programmer 생성자의 prototype과 Person의 객체를 연결하여 Programmer 객체도 person의 메소드(객체 내의 함수) introduce를 사용할 수 있게 되었다. 
Programmer.prototype = new Person();

// 상속받은 로직(method,property)에 새로운 method(coding) 추가. Programmer는 Person의 기능을 가지고 있으면서 Person이 가지고 있지 않은 기능인 메소드 coding을 가지고 있다. 
Programmer.prototype.coding = function(){
    return 'Hello World!'
}

function Designer(name){
    this.name = name;
}

// 상속받은 로직(method,property)에 새로운 method(coding) 추가. 
Designer.prototype = new Person();

Designer.prototype.design = function(){
    return 'Betiful World!'
}

const p1 = new Programmer('Terry');
console.log(p1.introduce());
console.log(p1.coding());

const p2 = new Designer('CREAMer');
console.log(p2.introduce());
console.log(p2.design());
```

특정 객체(object)를 상속 받기 위해서는 그 객체를 생성자(new)의 prototype에 할당을 하면 된다.

```javascript
Programmer.prototype = new Person();
```