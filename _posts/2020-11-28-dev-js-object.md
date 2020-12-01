---  
layout: post
title: JS - object 객체
categories: dev
tags: javascript
comments: true
---

Object 객체는 객체의 가장 기본적인 형태를 가지고 있는 객체이다. **다시 말해서 아무것도 상속받지 않는 순수한 객체**다. 자바스크립트에서는 값을 저장하는 기본적인 단위로 Object를 사용한다. 

**동시에 자바스크립트의 모든 객체는 Object 객체를 상속** 받는데, 그런 이유로 **모든 객체는 Object 객체의 프로퍼티를 가지고 있다.**

```javascript
Object.prototype.메소드 = functiont(){

}

function grandfather(){}
grandfather.prototype.lastname = 'lee';

function father(){}
father.prototype = new grandfather();

function me(){}
me.prototype = new father();

const son = new me();
console.log(son.lastname); // lee

// grandfather(){}, father(){}, me(){}는 모두 Object 객체를 상속 받으며 Object 프로퍼티를 가지고 있다.
```

### Object API 사용법
https://youtu.be/5OFbsj-UuIc?t=490

- Object.메소드() : prototype이 없는 메소드인 경우, Object.메소드(인자);
```javascript
const arr = [1, 2, 3]
Object.keys(arr);
// Object.keys = function{}
// Object는 생성자 함수. 
```

- Object.**prototype**.메소드() : prototype이 있는 메소드인 경우, 객체를 만들고 그 객체에 메소드를 넣어준다. 이와 같은 형태의 메소드는 객체를 생성하여 써야한다.
```javascript
const obj = new Object();
// 객체 생성
obj.toString();

const arr2 = new Array(1,2,3);
arr2.toString();
// Object.prototype.toString = function(){}
// 객체 함수
```

또한 Object 객체를 확장하면 모든 객체가 접근할 수 있는 API를 만들 수 있다. 

```javascript
Object.prototype.contain = function(neddle) {
    for(var name in this){
        if(this[name] === neddle){
            return true;
        }
    }
    return false;
}
var o = {'name':'egoing', 'city':'seoul'}
console.log(o.contain('egoing'));
var a = ['egoing','leezche','grapittie'];
console.log(a.contain('leezche'));
```
그런데 Object 객체는 확장하지 않는 것이 바람직하다. 왜냐하면 모든 객체에 영향을 주기 때문이다. Object 객체 확장은 신중하게 해야한다. 편리하면서 위험한 기능.

확장 후에 아래 코드를 실행해보자.
```javascript
for(var name in o){
    console.log(name);  
}
//name
//contain
```
확장한 프로퍼티인 contain이 포함되어 있다. 객체가 기본적으로 가지고 있을 것으로 예상하고 있는 객체 외에 다른 객체를 가지고 있는 것은 개발자들에게 혼란을 준다. 이 문제를 회피하기 위해서는 프로퍼티의 해당 객체의 소속인지를 체크해볼 수 있는 **hasOwnProperty**를 사용하면 된다. 
```javascript
for(var name in o){
    if(o.hasOwnProperty(name))
        console.log(name);  
}
```
**hasOwnProperty는 인자로 전달된 속성의 이름이 객체의 속성인지 여부를 판단**한다. 만약 prototype으로 상속 받은 객체라면 false가 된다. 