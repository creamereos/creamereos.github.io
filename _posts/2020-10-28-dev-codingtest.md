---  
layout: post  
title: JavaScript Coding Test : 가운데 글자 반환하기
subtitle: 
categories: dev  
tags: codingtest
comments: true  
--- 

# Coding Test
단어 s의 가운데 글자를 반환하는 함수, solution을 만들어 보세요. 단어의 길이가 짝수라면 가운데 두글자를 반환하면 됩니다.

- 제한사항 : s는 길이가 1 이상, 100이하인 스트링입니다.

# Pseudo Code

~~~
- 함수('function') solution을 정의한다.
- 반환('retrun') 할 변수를 정의해준다.
- ('if') 만약 solution 함수('function')안에 들어갈 s가 짝수(s % 2===0)라면
- {'concat'을 통해 가운데 두글자를 각각 인덱싱[]하여 스트링을 만들고 변수 answer에 담아준다.}
- ('else') 그렇지 않으면(홀수라면)
- {'concat'을 통해 가운데 한글자를 인덱싱해야한다.
- 홀수의 경우 소수점이 생기므로 가운데 글자 인덱싱을 위해서는 Math 메소드로 정수로 만들어준 후 변수 answer에 담아준다.
}
- 정의한 변수를 반환('return')한다.
~~~

# Concat
.concat() 메소드(method)는 문자열(string) 또는 배열(array)을 결합하여 새로운 문자열(string) 또는 배열(array)

기존 문자열(string) 또는 배열(array)은 변환하지 않고 새로운 문자열(string) 또는 배열(array)을 반환한다.

(대체할 만한 다른 함수들이 많아서 잘 사용 하지 않음)

* []인덱싱을 통해 특정 문자를 뽑아 낼 수 있다.

- Syntax

~~~
.concat()
~~~

- EX Code

~~~
[String EX Code]
var str1 = "Hello ";
var str2 = "world!";
var res = str1.concat(str2);

res : Hello World!

[Array EX Code]
var arr1 = ["1", "2"];
var arr2 = ["3", "4", "5"];
var res = ex1.concat(ex2);

res : [1,2,3,4,5]
~~~

# Math

- Math.round() 반올림

~~~
Math.round(4.7);    // returns 5
Math.round(4.4);    // returns 4
~~~

- Math.pow() 제곱

~~~
Math.pow(8, 2);      // returns 64
Math.pow(3, 4);      // returns 81
~~~

- Math.sqrt() 루트

~~~
Math.sqrt(64);      // returns 8
Math.sqrt(4);      // returns 2
~~~

- Math.abs() 정수

~~~
Math.abs(-4.7);     // returns 4.7
~~~

- Math.ceil() 올림

~~~
Math.ceil(4.4);     // returns 5
~~~

- Math.floor() 내림

~~~
Math.floor(4.7);    // returns 4
~~~

- Math.min() and Math.max()

~~~
Math.min(0, 150, 30, 20, -8, -200);  // returns -200

Math.max(0, 150, 30, 20, -8, -200);  // returns 150
~~~

- Math.random()

~~~
Math.random();     // returns a random number
~~~

# 정답

~~~
function solution(s) {
    var answer = '';

    if(s.length % 2 ===0){
        answer = answer.concat(s[s.length]/2 -1);
        answer = answer.concat(s[s.length]/2);
    } else {
        answer = answer.concat(s[Math.floor(s.length/2)]);
    }
    return answer;
}
~~~
