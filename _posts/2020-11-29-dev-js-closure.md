---  
layout: post
title: JS - closure
categories: dev
tags: js
comments: true
---

**클로저(closure)**는 내부함수가 외부함수의 **맥락(context)에 접근**할 수 있는 것을 가르킨다. 

### 내부 함수 : 함수 안에 있는 함수
외부 함수 : 함수를 포함하고 있는 함수
- 자바스크립트는 function 안에서 또 다른 function을 선언 가능.

```javascript
function 외부함수(){
    function 내부함수(){
        const title ='hello world';
        console.log(title);
    }
    내부함수();
}
외부함수();
// hello world
// 내부 함수는 외부 함수의 지역 변수(title)에 접근 가능.
// 즉 외부 함수 호출 시 외부 함수 안에 있는 내부 함수를 실행하는 내부함수를 호출 한다.
```

위 같은 코드는 함수 안에서만 실행되는 함수를 실행할 때 사용.

### 클로저

내부함수는 외부함수의 지역변수에 접근 할 수 있는데 외부함수의 실행이 끝나서 **외부함수가 소멸된 이후에도 내부함수가 외부함수의 변수에 접근 할 수 있다. 이러한 메커니즘을 클로저**라고 한다. 

```javascript
function 외부함수(){
    const title = 'hello world';
    return function(){
        // 위 함수는 즉시 실행 하는 익명함수다.
        console.log(title);
        
    }
}
내부 = 외부함수();
// 외부함수 호출을 변수 '내부'에 담긴다. 그 결과는 익명 함수다.
내부함수();
// hello world
```
