---  
layout: post
title: JS - 생성자와 new
categories: dev
tags: javascript
comments: true
---

**object(객체)** : 서로 연관된 **property(변수)**와 **function(함수)**를 **그룹핑한 {}그릇**

- property : 객체 내의 변수

- method : 객체 내의 function

### 객체 생성
객체 : {}

- 객체 생성 방법 1

```javascript
const perosn = {}
// perosn라는 변수에 비어있는{} objeect 생성
perosn.name = 'CREAMer';
//person에 name이란 property 변수 지정
person.introduce = function(){
    return 'My name is '+this.name;
    // this는 함수가 속해있는 변수 person를 담고 있는 객체 가리킨다.
}
//person에 introduce란 method 함수 지정
console.log(person.introduce());
// person.introduce() 메소드를 실행.

// My name is CREAMer
```

- 객체 생성 방법 2
위 코드는 객체를 만드는 과정에 분산되어 있다. 객체를 정의 할 때 값을 셋팅하도록 코드를 바꿀 수도 있다. 위 코드는 아래코드보다 가독성이 좋다.


```javascript
const person = {
    // 변수 person에 객체 생성
    'name' : 'CREAMer',
    //property : value
    'introduce' : function(){
    // property : method 
        return 'My name is '+this.name;
    }
}
document.write(person.introduce());
```

만약 다른 사람의 이름을 담을 객체가 필요하다면 객체의 정의를 아래 코드 처럼 반복해야 할 것이다. 

```javascript
const person1 = {
    // 변수 person에 객체 생성
    'name' : 'CREAMer',
    //property : value
    'introduce' : function(){
    // property : method 
        return 'My name is '+this.name;
    }
}
document.write(person.introduce());

const person2 = {
    // 변수 person에 객체 생성
    'name' : 'Terry',
    //property : value
    'introduce' : function(){
    // property : method 
        return 'My name is '+this.name;
    }
}
document.write(person.introduce());
```
코드가 중복된다. 가독성이 떨어지고, 코드의 양이 많아지며, 유지보수가 어려워진다.

이를 해결하기 위해 객체의 구조를 재활용할 수 있는 방법이 필요하다. 이 때 사용하는 것이 **생성자**다.

### 생성자 (Constructor)

**생성자(Constructor)** : 객체를 만드는 역할을 하는 함수. 자바스크립트에서 함수는 재사용 가능한 로직의 묶음이 아니라 객체를 만드는 창조자!

**new**가 붙은 함수는 **생성자**라고 부른다. (객체의 생성자)

```javascript
const A = new 함수();
A = 객체{};

함수에 new를 붙이면 그 값은 객체가 된다.
```

```javascript
function Person(name){
    // 생성자 함수는 첫글자를 대문자.
    this.name = name;
    this.introduce = function(){
        return 'My name is '+this.name; 
    }   
}
var p1 = new Person('CREAMer');
// 초기화 : 생성자 내에서 이 객체의 프로퍼티를 정의
document.write(p1.introduce()+"<br />");
 
var p2 = new Person('Terry');
// 초기화 : 생성자 내에서 이 객체의 프로퍼티를 정의

document.write(p2.introduce());
```

생성자 내에서 이 객체의 프로퍼티를 정의하고 있다. 이러한 작업을 **초기화**라고 한다. 이를 통해서 코드의 재사용성이 대폭 높아졌다.

코드를 통해서 알 수 있듯이 **생성자 함수는 일반함수와 구분하기 위해서 첫글자를 대문자**로 표시한다.

### 자바스크립트 생성자의 특징
함수에 new를 붙이는 것을 통해서 객체를 만들 수 있다는 점은 자바스크립트에서 함수의 위상을 암시하는 단서이면서 또 자바스크립트가 추구하는 자유로움을 보여주는 사례라고 할 수 있다.