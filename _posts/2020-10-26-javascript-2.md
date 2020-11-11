---  
layout: post
title: nodeJS - 변수
subtitle: 변수, var, const, let
categories: dev
tags: javascript
comments: true  
--- 

### var
보통 자바스크립트를 배울 때 var로 변수를 선언하는 방법을 배우지만 이제 var는 사용하지 않고 **const**와 **let**으로 대체.

~~~
if (true){
  var x = 3;
}
console.log(x);
콘솔 값 : 3
~~~
var는 함수 스코프를 가지므로 if문의 블록과 관계 없이 접근 가능

### const, let
const와 let은 **블록 스코프(범위)**를 갖는다.

**블록 스코프(범위)** : if,while,for,function 등에서 볼 수 있는 **중괄호{} 사이**

~~~
if (true){
  const y = 3;
}
console.log(x);
오류 발생 : Uncaught ReferenceError: y is not defined.  
~~~
**const**와 **let**은 **블록 스코프**를 가지므로 블록 밖에서는 변수에 접근 불가능.

**const** : 변수에 한번 값을 할당하면 다른 값을 할당 불가능. 다른 값을 할당하려고하면 에러 발생. 또한 초기화 할때 값을 할당하지 않으면 에러 발생. const로 선언한 변수를 **상수** 라고도 함. **기본적으로 const를 사용**

**let** : **다른 값을 할당해야 하는 상황이 생겼을 때 let 사용**
