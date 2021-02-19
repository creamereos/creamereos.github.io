---  
layout: post
title: Node JS - 템플릿 문자열
subtitle:
categories: dev
tags: js
comments: true  
--- 


# 템플릿 문자열
템플릿 문자열은 큰따옴표(")나 작은 따옴표(")로 감싸는 기존 문자열과 다르게 Tap 키 위에 있는 **백틱(`)** 으로 감싼다.

**${변수} 형식으로 변수를 더하기 기호 없이 문자열에 넣을 수 있다.**

~~~
`${변수1} ${변수2}... `
~~~

기존 따옴표 대신 백틱을 사용하므로 큰따옴표나 작은 따옴표도 함께 사용 가능.

~~~
`{num1} 더하기 {num2}는 "{result}"`
~~~


### ES5 템플릿 문자열 문법

- 가독성이 좋지 않음 (띄어쓰기, 변수, 더하기 기호 등)
- 작은 따옴표로 이스케이프(escape) 하느라 코드가 지저분하다.

~~~
var num1 = 1;
var num2 = 2;
var result = 3;

var string1 = num1 + ' 더하기 ' + num2 + ' 는 \''\';
console.log(string1);

// 1 더하기 2는 '3'
~~~

### ES2015 플러스 사용법

~~~
const num1 = 1;
const num2 = 2;
const result = 3;

const string = `{num1} 더하기 {num2}는 '{result}'`
console.log(string);

//문자열 안에 변수를 넣을 수 있다.
// ${변수} 형식으로 변수를 더하기 기호 없이 문자열에 넣을 수 있다. 기존 따옴표 대신 백틱을 사용하므로 큰따옴표나 작은 따옴표도 함께 사용 가능.
// 1 더하기 2는 '3'
~~~
