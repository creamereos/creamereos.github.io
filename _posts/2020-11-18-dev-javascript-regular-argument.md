---  
layout: post
title: JS - arguments (객체)
categories: dev
tags: js
comments: true
---

함수에는 arguments라는 변수에 담긴 숨겨진 유사 배열(배열과 비슷하지만 배열이 아님)이 있다.  이 배열에는 함수를 호출할 때 입력한 인자가 담겨있다. 

```javascript
function sum(매개변수){
    let i, _sum = 0;    
    for(i = 0; i < arguments.length; i++){
        document.write(i+' : '+arguments[i]+'<br />');
        _sum += arguments[i];
    }   
    return _sum;
}
document.write('result : ' + sum(1,2,3,4 인자));
```

- 매개변수(parameter)와 인자(argument)의 차이

```javascript
function 함수명(parameter매개변수) {

}
함수명(argument인자)
```

### 매개변수의 수

```javascript
function zero(){
    console.log(
        'zero.length', zero.length,
        'arguments', arguments.length
    );
}
zero(); // zero.length 0 arguments 0 

function one(arg1){
    console.log(
        'one.length', one.length,
        'arguments', arguments.length
    );
}
one('val1', 'val2');  // one.length 1 arguments 2 

function two(arg1, arg2){
    console.log(
        'two.length', two.length,
        'arguments', arguments.length
    );
}
two('val1');  // two.length 2 arguments 1
```