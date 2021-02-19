---  
layout: post
title: JS - 값(object) function callback
categories: dev
tags: js
comments: true
---

**JavaScript**에서는 **함수도 객체**다. 다시 말해서 일종의 값이다. 거의 모든 언어가 함수를 가지고 있다. JavaScript의 함수가 다른 언어의 함수와 다른 점은 **함수가 값**이 될 수 있다는 점이다. 

```javascript
function a(){}
// 위 코드는 아래 코드와 같다.
const a = function(){}
// 함수 a는 변수 a에 담겨진 값. 
```
- **메소드(method)** : 객체{}의 속성(property) 값(value)으로 담겨진 함수. 즉 객체에 속해있는 함수를 method라고 한다.

```javascript
a = {
    b:function(){
    // b=key(변수의 역할=property-속성) : function=value(method)
    }
};
//함수(b)는 객체의 value로 포함될 수 있다.
```

fucntion은 값(value)이기 때문에 다른 function의 인자(argument)로 전달 될수도 있다. 

```javascript
function cal(func, num){
    return func(num)
}
function increase(num){
    return num+1
}
function decrease(num){
    return num-1
}
alert(cal(increase, 1));
alert(cal(decrease, 1));
```

- 함수는 함수의 리턴 값으로도 사용할 수 있다.

```javascript
function cal(mode){
    var funcs = {
        'plus' : function(left, right){return left + right},
        'minus' : function(left, right){return left - right}
    }
    return funcs[mode];
}
alert(cal('plus')(2,1));
alert(cal('minus')(2,1));  
```

```javascript
function cal(mode){
    var funcs = {
        'plus' : function(left, right){return left + right},
        'minus' : function(left, right){return left - right}
    }
    return funcs[mode];
}
alert(cal('plus')(2,1));
alert(cal('minus')(2,1));   
```

- 배열의 값으로도 사용할 수 있다.

```javascript
var process = [
    function(input){ return input + 10;},
    function(input){ return input * input;},
    function(input){ return input / 2;}
];
var input = 1;
for(var i = 0; i < process.length; i++){
    input = process[i](input);
}
alert(input);
// 60.5
```

### 콜백
값으로 사용될 수 있는 특성을 이용하면 함수의 인자로 함수로 전달할 수 있다. 값으로 전달된 함수는 호출될 수 있기 때문에 이를 이용하면 함수의 동작을 완전히 바꿀 수 있다. 인자로 전달된 함수 **콜백 함수**의 구현에 따라서 sort의 동작방법이 완전히 바뀌게 된다.

쉽게 말해 **함수를 값으로 불러와서 쓰는 것**.

```javascript
function 함수명(a,b){
    return b-a;
}
let numbers = [20, 10, 9,8,7,6,5,4,3,2,1];
alert(numbers.sort(콜백 함수)); // array, [20,10,9,8,7,6,5,4,3,2,1]
```

### 비동기 처리 콜백
- 동기적 처리 : 작업 순서대로

- 비동기적 처리 : 작업 효율적으로

콜백은 비동기처리에서도 유용하게 사용된다. 시간이 오래걸리는 작업이 있을 때 이 **작업이 완료된 후에 처리해야 할 일을 콜백으로 지정**하면 **해당 작업이 끝났을 때 미리 등록한 작업을 실행**하도록 할 수 있다.
https://opentutorials.org/course/743/6508
