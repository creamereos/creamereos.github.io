---  
layout: post
title: forEach 기본
subtitle:
categories: dev
tags: javascript
comments: true  
--- 

forEach() method는 주어진 callback function을 array에 있는 각 요소에 대해 오름차순으로 한 번씩 반복해서 실행한다.삭제했거나 초기화하지 않은 인덱스 속성에 대해서는 실행하지 않는다. forEach()를 중간에 멈출 수 없다. 중간에 멈춰야 한다면 forEach()가 적절한 방법이 아닐지도..

- syntax

```javascript
배열.forEach(콜백함수(처리할 element[]))

arr.forEach(callback(currentvalue[, index[, array]])[, thisArg])

// []안에 있는건 선택사항 ex)[, index[, array]])[, thisArg]
```

- ex code1: forEach() 메서드는 주어진 함수를 배열 요소 각각에 대해 실행

```javascript
const 배열1 = ['a', 'b', 'c'];

배열.forEach(배열 요소 변수명 => console.log(배열요소 변수명));
// a, b, c
```

- ex code2: forEach 화살표 함수 사용

```javascript
const 배열2 = [0,1,2,3,4,5,6,7,8,9,10];

배열2.forEach(function(배열 요소 변수명){
    console.log(배열 요소 변수명); 
    // 0 1 2 3 4 5 6 7 8 9 10
});

// arrow 함수 사용 가능
배열2.forEach(배열 요소 변수명 => console.log(배열 요소 변수명));
```

- ex code3: for문을 forEach 문으로 (홀수만 뽑아내기)

```javascript
const 배열2 = [0,1,2,3,4,5,6,7,8,9,10];
const 배열3 = [];

// 이전
for (let i=0; i<items.length; i++) {
  if (i % 2 !== 0) {
    배열3.push(배열2[i]);
  }
}

// 이후
items.forEach(function(배열2 요소의 변수){
  if (배열 2 요소의 변수 % 2 !==0){
    배열3.push(배열 2 요소의 변수);
  }
});
```

- ex code4 : forEach의 callback 함수에는 array의 elements 뿐만아니라 index, 전체 array를 인자로 사용 가능

```javascript
const 배열2 = [0,1,2,3,4,5,6,7,8,9,10];

배열2.forEach(function(element, index, array){
    console.log(`${array}의 ${index}번째 요소 : ${element}`);
});

// 0,1,2,3,4,5,6,7,8,9,10의 0번째 요소 : 0
// 0,1,2,3,4,5,6,7,8,9,10의 1번째 요소 : 1
// 0,1,2,3,4,5,6,7,8,9,10의 2번째 요소 : 2
// 0,1,2,3,4,5,6,7,8,9,10의 3번째 요소 : 3
// 0,1,2,3,4,5,6,7,8,9,10의 4번째 요소 : 4
// 0,1,2,3,4,5,6,7,8,9,10의 5번째 요소 : 5
// 0,1,2,3,4,5,6,7,8,9,10의 6번째 요소 : 6
// 0,1,2,3,4,5,6,7,8,9,10의 7번째 요소 : 7
// 0,1,2,3,4,5,6,7,8,9,10의 8번째 요소 : 8
// 0,1,2,3,4,5,6,7,8,9,10의 9번째 요소 : 9
// 0,1,2,3,4,5,6,7,8,9,10의 10번째 요소 : 10
```

- ex code5 : forEach는 callback 함수 이외에 thisArg라는 객체(Object) 인자도 사용 가능

```javascript
const arr = [0,1,2,3,4,5,6,7,8,9,10];
const obj1 = {name: "kim"};
const obj2 = {name: "park"};

arr.forEach(function(element){
    console.log(`${this.name} - ${element}`);
}, obj1);
/*
kim - 0
kim - 1
kim - 2
kim - 3
kim - 4
kim - 5
kim - 6
kim - 7
kim - 8
kim - 9
kim - 10
*/
arr.forEach(function(element){
    console.log(`${this.name} - ${element}`);
}, obj2);
/*
park - 0
park - 1
park - 2
park - 3
park - 4
park - 5
park - 6
park - 7
park - 8
park - 9
park - 10
*/
```