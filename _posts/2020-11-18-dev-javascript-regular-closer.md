---  
layout: post
title: JS - closure
subtitle:
categories: dev
tags: js
comments: true
---

클로저 다시 공부하기. 어렵다 ㅠ

클로저(closure)는 내부함수가 외부함수의 맥락(context)에 접근할 수 있는 것을 가르킨다. 

# 내부 함수
- 자바스크립트는 함수 안에서 또 다른 함수를 선언 가능

```javascript
function 외부함수(){
    function 내부함수(){
        var title = 'hello world'; 
        alert(title);
    }
    내부함수();
}
외부함수();
```

- 내부함수는 외부함수의 지역변수에 접근할 수 있다.

```javascript
function 외부함수(){
    let title = 'Hello World';  
    function 내부함수(){
        alert(title);
    }
    내부함수();
}
외부함수();
```

### 클로저

- 클로저 : 내부함수는 외부함수의 지역변수에 접근 할 수 있는데 외부함수의 실행이 끝나서 외부함수가 소멸된 이후에도 내부함수가 외부함수의 변수에 접근

```javascript
function outter(){
    let title = 'coding everybody';  
    return function(){        
        alert(title);
    }
}
let inner = outter();
inner();
```
- 클로저 예제

```javascript
function factory_movie(title){
    return {
        get_title : function (){
            return title;
        },
        set_title : function(_title){
            title = _title
        }
    }
}
ghost = factory_movie('Ghost in the shell');
matrix = factory_movie('Matrix');
 
alert(ghost.get_title());
alert(matrix.get_title());
 
ghost.set_title('공각기동대');
 
alert(ghost.get_title());
alert(matrix.get_title());
```

```javascript

```

```javascript

```

```javascript

```

```javascript

```