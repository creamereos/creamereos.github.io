---  
layout: post
title: JS Coding Test - 문자열 내 마음대로 정렬하기
subtitle: 
categories: dev
tags: codingtest
comments: true  
--- 

[문자열 내 마음대로 정렬하기](https://programmers.co.kr/learn/courses/30/lessons/12915)

# Coding Test
문자열로 구성된 리스트 strings와, 정수 n이 주어졌을 때, 각 문자열의 인덱스 n번째 글자를 기준으로 오름차순 정렬하려 합니다. 예를 들어 strings가 [sun, bed, car]이고 n이 1이면 각 단어의 인덱스 1의 문자 u, e, a로 strings를 정렬합니다.

[제한 조건]
- strings는 길이 1 이상, 50이하인 배열입니다.
- strings의 원소는 소문자 알파벳으로 이루어져 있습니다.
- strings의 원소는 길이 1 이상, 100이하인 문자열입니다.
- 모든 strings의 원소의 길이는 n보다 큽니다.
- 인덱스 1의 문자가 같은 문자열이 여럿 일 경우, 사전순으로 앞선 문자열이 앞쪽에 위치합니다.

[입출력 예]

~~~
ex1)
strings : [sun, bed, car]
n : 1
return : [car, bed, sun]

ex2)
strings : [abce, abcd, cdx]
n : 2
return : [abcd, abce, cdx]
~~~


# Solution

~~~
function solution(strings, n) {
    strings.sort(function(a,b){

        if(a[n] > b[n]) return 1;
        if(a[n] < b[n]) return -1;

        if(a > b) return 1;
        if(a < b) return -1;
        if(a == b) return 0;
    });
    return strings;
}
~~~

# Study

### localeCompare

### sort
sort method는 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환.
기본 정렬 순서는 문자열의 유니코드 포인트를 따른다.

- Syntax

~~~
arr.sort([compareFunction])

arr.sort()
compareFunction이 제공되지 않으면 요소를 문자열로 변환하고 유니코드 포인트 순서로 문자열을 비교하여 정렬.

function compare(a,b) {
	if(a < b) return -1; // a b 순 정렬 .
  if(a > b) return 1; // b a 순 정렬.
  if(a == b) return 0; // 변경하지 않음.
}
~~~

- 오름차순 정렬

~~~
function compareNumbers(a, b) {
  return a - b;
}
~~~

- 내림차수 정렬

~~~
function compareNumbers(a, b) {
  return b - a ;
}
~~~
