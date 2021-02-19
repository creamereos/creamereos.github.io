---  
layout: post
title: JS - prototype
categories: dev
tags: js
comments: true
---
함수(function)는 객체(object{})다. 그러므로 생성자(new, constructor)로 사용될 함수도 객체다. 객체는 property를 가질 수 있는데 **prototype**이라는 property는 그 용도가 약속되어 있는 특수한 property다. 

 **prototype에 저장된 속성들은 생성자를 통해서 객체가 만들어질 때 그 객체에 연결**된다. 

- prototype chain : prototype은 객체와 객체를 연결하는 체인의 역할을 하는 것이다.

```javascript
function grandfather(){}
grandfather.prototype.lastname = 'lee';

function father(){}
father.prototype = new grandfather();

function me(){}
me.prototype = new father();

const son = new me();
console.log(son.lastname); // lee

// 계속 prototype을 찾으면서 lastname 값을 가진 것을 찾는다.

// 생성자 me를 통해서 만들어진 객체 son이 grandfather의 property인 lastname에 접근 가능한 것은 prototype 체인으로 son과 grandfather가 연결되어 있기 때문이다. 내부적으로는 아래와 같은 일이 일어난다.

// 1.객체 son에서 lastname을 찾는다.
// 2.없다면 me.ultraProp를 찾는다.
// 3.없다면 father.prototype.ultraProp를 찾는다.
// 4.없다면 grandfather.prototype.ultraProp를 찾는다.

```

- 주의 사항

```javascript
// 코드 예시 1 : 가능
me.porotype = new father();

// 위 코드는 father.prototype의 원형으로 하는 객체가 생성되기 때문에 new father()를 통해서 만들어진 객체에 변화가 생겨도 father.prototype의 객체에는 영향을 주지 않는다.

// 코드 예시 2 : 안됨.
me.prototype = father.prototype 

// 코드 예시 2번의 me.prototype의 값을 변경하면 그것이 father.prototype도 변경하기 때문이다.
```