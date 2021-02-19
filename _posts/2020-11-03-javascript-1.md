---  
layout: post
title: Node JS - 화살표 함수
subtitle: 
categories: dev
tags: js
comments: true  
--- 

# 화살표 함수

- function 선언 대신 => 기호로 함수를 선언 가능

- 화살표 함수에서 내부에 return 문 밖에 없는 경우에는 return 문을 줄 일 수 있다. 중괄호{} 대신 return 할 수식을 바로 적으면 된다.

- 변수에 대입하여 나중에 재사용 가능

~~~
function add(x,y) {
  return x + y;
}

const add = (x,y) => x+y;
~~~

#### 화살표 함수가 기존 function과 다른점 : this 바인드 방식
그냥 쉽게 **기본적으로 화살표 함수**를 쓰되 **this를 사용해야하는 경우**에는 **화살표 함수와 function 둘 중에서 하나**를 골라야한다.


- ES5

~~~
var relation1 = {
  name: 'zero',
  friends: ['nero', 'hero', 'xero'],

  logFriends: function() {
    var that = this; // relation1 함수를 가리키는 this를 that에 저장

    this.friends.forEach(function (friend) {
      console.log(that.name, friend);
      });
  },
};
relation1.logFriends();
~~~

- ES 2015 플러스

~~~
const relation2 = {
  name: 'zero',
  friends: ['nero', 'hero', 'xero'],

  logFriends: function() {
  //화살표 함수 사용
  this.friends.forEach(friend =>{
    console.log(this.name, friend);
    });
  },
};
~~~
