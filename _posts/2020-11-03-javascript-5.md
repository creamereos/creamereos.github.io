---  
layout: post
title: Node JS - 구조 분해 할당
subtitle:
categories: dev
tags: javascript
comments: true  
--- 

# 구조 분해 할당
구조 분해 할당을 사용하면 객체와 배열로부터 속성이나 요소를 쉽게 꺼낼 수 있다.

- 객체의 속성을 같은 이름의 변수에 대입하는 코드

ES 5

~~~
var candyMachine = {
  status: {
    name: 'node',
    count: 5,
  },
  getCandy: function() {
    this.status.count--;
    return this.status.count;
  },
};
var getCandy = candyMachine.getCandy;
var count = candyMachine.status.count;
~~~

ES 2015 플러스

~~~
cosnt candyMachine = {
  status: {
    name: 'node',
    count: 5,
  },
  getCandy() {
    this.status.count;
  }
};

const {getCandy, status: {count}} = candyMachine;
~~~

# 배열에 대한 구조분해 할당 문법

[ES 5]

~~~
var array = ['nodejs', {}, 10, true];
var node = array[0];
var obj = array[1];
var bool = array[3];
~~~

[ES2015 플러스]
구조분해 할당 문법도 코드 줄 수를 상당히 줄여줄수있다.

~~~
const array = ['nodejs', {}, 10, true];
const [node, obj, , bool] = array;

10은 변수명을 지어주지 않았으므로 무시.
~~~
