---  
layout: post
title: JS Coding Test - 두개 뽑아서 더하기
subtitle: 
categories: dev
tags: codingtest
comments: true  
--- 

# 문제 : 두개 뽑아서 더하기
정수 배열 numbers가 주어집니다. numbers에서 **서로 다른 인덱스**에 있는 두 개의 수를 뽑아 더해서 만들 수 있는 모든 수를 배열에 오름차순으로 담아 return 하도록 solution 함수를 완성해주세요.

### 제한사항
- numbers의 길이는 2 이상 100 이하입니다.
- numbers의 모든 수는 0 이상 100 이하입니다.

# Pseudo Code

~~~
1. function 'solution(numbers)'을 정의한다.
2. 배열에서 처음 뽑을 인덱스[]를 for문을 통해 순서대로 반복하여 뽑아준다.
3. 배열에서 두번째 뽑을 인덱스는 처음에 뽑은 인덱스가 포함될 수 있으니 이중 for문을 통해 반복한다.
4.뽑은 인덱스들을 더한다.
5.만약 배열에서 중복된 숫자가 없다면 넣어준다.
6.오름차순으로 담는다
7.배열을 반환한다.
~~~

### 이중 반복문(for)
- Syntax

~~~
for (statement 1; statement 2; statement 3) {
  for (statement 1; statement 2; statement 3) {
    반복 실행할 코드 블록 입력
  }
}
~~~

Statement 1: (선택 사항)코드 블록이 실행되기 전 한번만 실행. (일반적으로 i=0)

Statement 2: (선택 사항)코드가 실행 조건을 정의. 조건이 true면 루프 다시 시작. false면 루프 종료.(만약 Statement 2를 생략하면 반드시 루프 안에 break를 넣어줘야한다. 그렇지 않으면 반복문이 끝나지 않음)

Statement 3: 코드 블록이 실행된 후 초기 변수 값을 증/감하여 매번 실행된다.(i--,i++,i=i+1.. etc)

*Statement 1,2,3 모두 선택 사항이며 상황에 따라 생략 가능하다.

- EX Code

~~~
구구단
for(var i=2; i<10; i++){
    for(var j=1; j<10; j++){
        let sum = i * j;
      }
    }
~~~

### 중복 Array 제거
- Set
Set 은 unique 값만 저장할 수 있도록 하기 때문에 array에 넣게 되면, 중복되는 값이 사라짐. 또한 array 에 Set 을 이용하는 것 대신에 Array.from 도 가능.

~~~
const array = ['0', 1, 2, '0', '0', 3]
Array.from(new Set(array));

반환(return) ['0', 1, 2, 3]
~~~

- Filter
filter는 array 내의 각 element 에 조건을 주어, true 값을 return 한 element 만 모아서 새로운 array 를 생성

~~~
const array = ['0', 1, 2, '0', '0', 3]
array.filter(item, index) => array.indexOf(item) == index);

반환(return) ['0', 1, 2, 3]

indexOf 는 array 안에서 값을 제일 먼저 찾은 위치.
~~~

### Array에 없는 것만 포함시키기

~~~
const array = ['0', 1, 2, '0', '0', 3]
let answer = [];

for(i=0; i<array.length; i++){
  if(!answer.includes(array[i])){
    answer.push(array[i]);
  }
}

반환(return) ['0', 1, 2, 3]
~~~

### Array Method

- includes
includes 는 array속 해당 element가 있으면 true / 없으면 false를 return

~~~
var a = [1,2,3,4,5,1,2,3]

a.includes(3)
//true

a.includes(6)
//false
~~~

- indexOf
찾은 값의 첫번째 element의 위치를 return해주며, 없을경우 -1을 리턴

~~~
let a = [1,2,3,4,5,1,2,3]

a.indexOf(3)
//2

a.indexOf(6)
//-1
~~~

- pop
array의 마지막 element 제거, 제거된 element 리턴

~~~
var arr = [1, 2, 3];
arr.pop();

반환(return) 3
~~~

- push
array의 끝에 element를 추가

~~~
var arr = ['a', 'b', 'c'];
arr.push('d');

arr = ['a', 'b', 'c', 'd']
~~~

### 배열 오름차순, 내림차순

- 문자 정렬 (오름차순)

~~~
var fruit = ['orange', 'apple', 'banana'];

fruit.sort();

return : apple, banana, orange
~~~

- 숫자 정렬 (오름차순)

~~~
var arr = [4, 11, 2, 10, 3, 1];

arr.sort(function(a, b) {return a - b;});

return : 1, 2, 3, 4, 10, 11
~~~

- 숫자 정렬 (내림차순)

~~~
var arr = [4, 11, 2, 10, 3, 1];

arr.sort(function(a, b) {return b - a;});

return : 11, 10, 4, 3, 2, 1
~~~

# Solution

~~~
function solution(numbers){
  let answer = [];
  let i;
  let j;
  let n = numbers.length;

  for(i=0; i<n; i++){
    for(j=i+1; j<n; j++){
      let sum = numbers[i]+numbers[j];
        if(!answer.includes(sum)){
          answer.push(sum);
        }
    }
  }
answer.sort(function(a, b){return a - b;});
return answer
}
~~~
