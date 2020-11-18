---  
layout: post
title: JS - 함수의 호출 (apply 메소드)
categories: dev
tags: javascript
comments: true
---

- 함수 호출 기본적인 방법

```javascript
function 함수명(){
}
함수명();
```

- 함수명.apply 사용 방법

```javascript
function sum(arg1, arg2){
    return arg1+arg2;
}
sum(1,2)
//3
alert(sum.apply(null, [1,2])
)
//3
```

- 함수명.apply 사용하는 이유

```javascript
o1 = {val1:1, val2:2, val3:3}
o2 = {v1:10, v2:50, v3:100, v4:25}
function sum(){
    var _sum = 0;
    for(name in this){
        _sum += this[name];
    }
    return _sum;
}
alert(sum.apply(o1)) // 6
alert(sum.apply(o2)) // 185
```
https://youtu.be/Ubs30Xxe-Ps