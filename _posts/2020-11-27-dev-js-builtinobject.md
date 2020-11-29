---  
layout: post
title: JS - 표준 내장 객체(standard built-in object)
categories: dev
tags: JS
comments: true
---
- 표준 내장 객체 : javascript가 기본적으로 제공하는 객체

```javascript
Object
Function
Array
String
Boolean
Number
Math
Date
RegExp
```

- 사용자 정의 객체 : 사용자가 직접 정의하는 객체

### 내장 객체에 사용자 정의 객체 추가하기
javascript가 기본적으로 제공하는 객체에 사용자가 직접 정의하는 객체 추가

```javascript
const arr = new Array(1,2,3,4,5,6);

function dice(arr){
    const index = Math.floor(arr.length*Math.random());
    return arr[index];
}
console.log(dice(arr));
// 함수명은 함수명만 보고 함수의 역할을 알수 있도록 명확하게 지정하는게 좋다.
```

-  함수를 배열 객체에 포함. 그렇게하면 마치 배열에 내장된 메소드인 것처럼 위의 기능을 사용할 수 있다.

This 활용. Array의 prototype에 random이란 method 추가.

```javascript
// 함수를 Array 객체에 포함 시키고 random이라는 method를 prototype을 통해 Array 객체에 넣어준다. 
Array.prototype.random = function(){
    const index = Math.floor(this.length*Math.random());
    return this[index];
}
// method random안에 있는 this는 객체 Array를 가리킨다.

// 객체 상속을 통해 Array의 변경된 값들에 위 객체 메소드를 사용할 수 있다.

const arr1 = new Array(1,2,3,4,5,6);

const arr2 = new Array(7,8,9,10,11,12);

console.log(arr1.random());
// 1~6 중 랜덤한 숫자

console.log(arr2.random());
// 7~12 중 랜덤한 숫자
```

