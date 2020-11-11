---  
layout: post
title: JS Coding Test - 수박수박수박수박수박...
subtitle: 
categories: dev
tags: codingtest
comments: true  
--- 

# Test
길이가 n이고, 수박수박수박수....와 같은 패턴을 유지하는 문자열을 리턴하는 함수, solution을 완성하세요. 예를들어 n이 4이면 수박수박을 리턴하고 3이라면 수박수를 리턴하면 됩니다.
https://programmers.co.kr/learn/courses/30/lessons/12922

<table>
  <th> 입력(n) </th>
  <th> 출력(return) </th>

  <tr>
    <td> 3 </td>
    <td> 수박수 </td>
  </tr>

  <tr>
    <td> 4 </td>
    <td> 수박수박 </td>
  </tr>
</table>

# Pseudo code

~~~
만약 n이 짝수면 '수박을 n/2만큼 곱해준다.
n이 홀수면 n/2로 나눈 나머지 값을 버리고 '수박'에 곱해준 후 '수'를 더해준다.
~~~

# Answer

~~~
function solution(n) {
    let answer = '';
    const str = "수박";
    const str1 = "수";

    if (n % 2 === 0) {
        answer = str.repeat(n/2);
    } else {
        answer = str.repeat(Math.floor(n/2))+str1;   
    }
    return answer;
}
~~~

# Other Answer

- for if(삼항 연산자)문으로 풀기

~~~
function waterMelon(n){
  var result = "";
    for(var i = 0 ; i < n ; i++) {
        result += i % 2 == 0 ? "수" : "박";
  }
  return result;
}
~~~

- 삼항 연산자
~~~
const waterMelon = n => {
    return '수박'.repeat(n/2) + (n%2 === 1 ? '수' : '');
}
~~~

- '**=>**'? slice

~~~
const waterMelon = n => "수박".repeat(n).slice(0,n);
~~~

# Study

### String 합치기
- 플러스(+) 연산자

~~~
const str = 'Hello' + ' ' + 'world';

document.writeln(str); // Hello world
~~~

~~~
 '+=' 연산자를 사용 가능
let str = 'Hello';
str += ' ';
str += 'world';

document.writeln(str); // Hello world
~~~

- Join() 함수

~~~
arr.join([separator])

* separator는 옵션
~~~

배열의 모든 요소를 연결해 하나의 문자열로 만듬.
배열의 모든 문자열을 이어 붙임

장점은 문자열을 이어 붙일 때 separator(구분자)를 지정해 줄 수 있음. join() 함수를 잘 활용하면 반복문을 사용하지 않고도, 문자열에 구분자를 넣어서 이어붙여 줄 수 있음.

~~~
const str1 = ['Hello', 'world'].join();
const str2 = ['Hello', 'world'].join('♥');

document.write(str1);
document.write('<br>');
document.write(str2);
//
Hello,world
Hello♥world
~~~

### 삼항 연산자
if문의 단축 형태

~~~
조건문 ? 선택문1(True) : 선택문2(False)

a > b ? "a가 b보다 크다" : a가 b보다 크지 않다"
~~~


### 화살표 함수 **=>**
화살표 함수(Arrow function)는 function 키워드 대신 화살표(=>)를 사용하여 보다 간략한 방법으로 함수를 선언할 수 있다. 하지만 모든 경우 화살표 함수를 사용할 수 있는 것은 아니다.

- 화살표 함수의 기본 문법은

~~~
// 매개변수가 없는 경우
var foo = () => console.log('bar');
foo(); // bar

// 매개변수가 하나인 경우
var foo = x => x;
foo('bar'); // bar

// 매개변수가 여려개인 경우
var foo = (a, b) => a + b; // 간단하게 한줄로 표현할 땐 "{}" 없이 값이 반환됩니다.
foo(1, 2); // 3

var foo = (a, b) => { return a + b };
foo(1, 2); // 3

var foo = (a, b) => { a + b }; // "{}"를 사용했는데 return이 없을 때
foo(1, 2); // undefined

var foo = (a, b) => { // 여러줄 썼을 때
	var c = 3;
	return a + b + c;
}
foo(1, 2, 3) // 6
/*
"{}"를 사용하면 값을 반환할 때 return을 사용해야합니다.
"{}"를 사용하지 않으면 undefied를 반환합니다.
"{}"을 사용할 때는 여러줄을 썼을 때 사용합니다.
*/

// 객체를 반환할 때
var foo = () => ( { a: 1, b: 2, c: 3 } );
foo(); // { a: 1, b: 2, c: 3 };
~~~

### slice
Array.slice() 메서드는 배열의 일부분(slice) 혹은 부분 배열(subarray)을 반환합니다.

slice() 메서드는 전달 인자를 두 개 받는데, 각 인자는 반환될 부분의 처음과 끝을 각각 명시합니다.

반환되는 배열은 첫 번째 전달인자가 지정하는 위치부터 두 번째 전달인자가 지정하는 위치를 제외한 그 사이의 모든 원소를 포함합니다.

만약 전달 인자를 하나만 명시하면, 그 위치에서 배열 끝까지의 모든 원소를 포함하는 부분 배열을 반환합니다.

만약 전달 인자가 음수라면 배열의 마지막 원소에서 상대적인 위치로 배열 원소를 지정합니다.

예를 들어, 전달 인자 -1은 배열의 마지막 원소를 가리키며, 전달인자 -3은 배열의 마지막 원소부터 앞쪽으로 세 번째 원소를 가리킵니다

~~~
var a = [1, 2, 3, 4, 5]; a.slice(0, 3); // [1, 2, 3]을 반환한다. a.slice(3); // [4, 5]를 반환한다. a.slice(1, -1); // [2, 3, 4]를 반환한다. a.slice(-3, -2) // [3]을 반환한다.
~~~
