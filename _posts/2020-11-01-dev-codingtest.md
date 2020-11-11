---  
layout: post
title: JS Coding Test - 같은 숫자는 싫어
subtitle: 
categories: dev
tags: codingtest
comments: true  
--- 

# Coding Test
배열 arr가 주어집니다. 배열 arr의 각 원소는 숫자 0부터 9까지로 이루어져 있습니다. 이때, 배열 arr에서 연속적으로 나타나는 숫자는 하나만 남기고 전부 제거하려고 합니다. 단, 제거된 후 남은 수들을 반환할 때는 배열 arr의 원소들의 순서를 유지해야 합니다. 예를 들면,

- arr = [1, 1, 3, 3, 0, 1, 1] 이면 [1, 3, 0, 1] 을 return
- arr = [4, 4, 4, 3, 3] 이면 [4, 3] 을 return

배열 arr에서 연속적으로 나타나는 숫자는 제거하고 남은 수들을 return 하는 solution 함수를 완성해 주세요.

# Pseudo code

~~~
배열 arr에 [0]번째 요소를 새로운 배열(answer)에 push준다.;
- [0]번째 요소는 중복되지 않으므로
for (배열 arr의 [1]번째 요소부터 n(<arr.length)번째까지) {반복.
if (arr의 n번째 요소 !== arr n-1 번째)이면
{arr의 배열(answer)에 push준다.;}}
~~~

# solution

~~~
function solution(arr) {
  let answer = [];
  answer.push(arr[0]);
  for(let i=1; i < arr.length; i++){
    if(arr[i] !== arr[i-1]){
      answer.push[i];
    }
  }
  return answer;
}
~~~

# other solution

~~~
function solution(arr){
    return arr.filter((val,index) => val != arr[index+1]);
}
~~~

# Study

### filter
filter() 함수는 특정 조건에 부합하는 배열의 모든 값을 배열 형태로 리턴.

- syntax

~~~
var newArray = arr.filter(callbackFunction(element, index, array), thisArg);

- element : 요소값
- index : 요소의 인덱스
- array : 사용되는 배열 객체
- thisArg : filter에서 사용될 this 값입니다. 선택적으로 사용되며 사용하지 않을 경우 undefined 전달.
~~~

~~~
var array1 = [-1,0,1]; array1.filter((num) => num > 0); // [1]

- 콜백 함수 호출 사용
var greaterThanZero = (num) => num > 0; var array1 = [-1,0,1]; array1.filter(greaterThanZero); // [1]
~~~


### callbackFunction
콜백은 다른 함수가 **실행을 끝낸 뒤 실행**되는 — call back 되는 함수.

[콜백 함수 자세히 알아보기](https://medium.com/@oasis9217/%EB%B2%88%EC%97%AD-javascript-%EB%8F%84%EB%8C%80%EC%B2%B4-%EC%BD%9C%EB%B0%B1%EC%9D%B4-%EB%AD%94%EB%8D%B0-65bb82556c56)

### 화살표 함수
함수 표현식보다 단순하고 간결한 문법으로 함수를 만들 수 있는 방법.

~~~
- 예시 함수1
let func = function(arg1, arg2, ...argN) {
  return expression;
};

- 화살표 함수1(예시 함수1의 축약형)
let func = (arg1, arg2, ...argN) => expression

- 예시 함수2
let sum = function(a, b) {
  return a + b;
};

- 화살표 함수2(예시 함수2의 축약형)
let sum = (a, b) => a + b;

(a, b) => a + b는 인수 a와 b를 받는 함수입니다. (a, b) => a + b는 실행되는 순간 표현식 a + b를 평가하고 그 결과를 반환.
~~~

### 비교 연산자
- 동등 연산자 : 자료형이 같지 않은 경우 같아지도록 변환한 후 같은 경우

~~~
==
1   ==  1        // true
"1"  ==  1        // true
1   == '1'       // true
0   == false     // true
0   == null      // false

0   == undefined // false
null  == undefined // true
~~~

- 부등 연산자
자료형(string,int)이 같지 않은 경우 같아지도록 변환한 후 같지 않은 경우

~~~
1 !=   2     // true
1 !=  "1"    // false
1 !=  '1'    // false
1 !=  true   // false
0 !=  false  // false
~~~

- 일치 연산자
자료형 변환 없이 두 연산자가 같은 경우

~~~
3 === 3   // true
3 === '3' // false
~~~

- 불일치 연산자
자료형 변환 없이 두 연산자가 같지 않은 경우

~~~
3 !== '3' // true
4 !== 3   // true
~~~

### 관계 연산자

~~~
=은 반드시 <, > 다음에 써준다.
<
>
<=
>=
~~~

### 자료형 데이터 기본 타입

- Number : 숫자

- String : 문자 ('')

- Boolean : True / False

- undefined : 값을 할당하지 않은 변수

- Null : null

### 배열의 함수

- push : 배열의 끝에 원하는 값을 추가

~~~
var example = new Array("a", "b", "c");
example.push("d");

document.write(example);
//결과값 a,b,c,d
~~~

-  pop : 배열의 마지막에 있는 값을 제거

~~~
var example = new Array("a", "b", "c");
example.pop();

document.write(example);
//결과값 a,b
~~~

- shift : 배열의 첫번째에 있는 값을 제거하여 반환

~~~
var example = new Array("a", "b", "c");
example.shift();

document.write(example);
//결과값 b,c
~~~

- length : 배열의 길이를 반환

~~~
var example = new Array("a", "b", "c");

document.write(example.length);
//결과값 3
~~~

- concat : 2개의 배열을 합쳐주는 기능.

~~~
var example = new Array("a", "b", "c");
var example2 = new Array("d","e","f");

example = example.concat(example2);
document.write(example);
//결과값 a,b,c,d,e,f
~~~

- join : 배열값 사이에 원하는 문자를 삽입

~~~
var example = new Array("a", "b", "c");
example = example.join("/");

document.write(example);
//결과값 a/b/c
~~~

- reverse : 배열을 역순으로 재배치

~~~
var example = new Array("a", "b", "c");
example.reverse();

document.write(example);
//결과값 c,b,a
~~~

- sort : 배열 정렬.

~~~
var example = new Array(1,4,2,3,5);
example.sort();

document.write(example);
//결과값 1,2,3,4,5
~~~

- slice : 배열의 일부분을 반환

~~~
var example = [1, 2, 3, 4];
var example2 = example. slice(0, -1);

document.write(example);
document.write("<br/>");

example2 = example. slice(-2);
document.write(example2);

//결과값
//1,2,3,4
//3,4
~~~

- splice : 배열값을 추가하거나 제거하여 반환

~~~
var example = ["a", "b", "c", "d"];
var example2 = example.splice(1, 2);

document.write(example);
document.write("<br/>");
document.write(example2)

//결과값
//a,d
//b,c
~~~
