---  
layout: post
title: Node JS -  객체 리터럴
subtitle:
categories: dev
tags: javascript
comments: true  
--- 

# 객체 리터럴

### 객체(obeject)란?
객체 란 여러 속성을 하나의 변수에 저장할 수 있도록 해주는 데이터 형식입니다. 객체를 만들기 위해서는 평소처럼 변수를 선언한 다음, 중괄호로 키-값의 속성쌍을 묶어줍니다.

~~~
ex :
let objectName = {
  propertyName: propertyValue,
  propertyName: propertyValue,
  ...
};
~~~

### 리터럴(literal)이란?
정의: 모든 데이터 타입에 들어가는 데이터값 그 자체.

~~~
const plus = {a1: 1, a2: 2};

console.log(plus); // { a1: 1, a2: 2 }

중괄호 {}부분이 객체 리터럴. 각각의 프로퍼티는 쉼표(,)로 구분이 가능합니다. 그리고 프로퍼티 이름을 지금은 a1이라고 했지만 "a1"으로 해도 상관은 없음.
~~~

### 플러스 객체 리터럴 특징

- 속성명과 변수명이 동일한 경우 한번만 써줘도 된다. (코드의 중복을 피할 수 있음)

~~~
[ES 5]
{name : name, age: age}

[ES 2015 플러스]
{name, age}
~~~

- 객체의 메서드에 함수를 연결 할 때 더는 콜론과 function을 붙이지 않아도 된다.

- 객체의 속성명을 동적(함수에 정의하고 함수 밖에서도 사용)으로 생성 가능
