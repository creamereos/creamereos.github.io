---  
layout: post  
title: JS 21.03.11
subtitle: Primitive vs Reference
categories: dev
tags: js
comments: true  
--- 

참고 : https://okayoon.tistory.com/entry/%EC%95%84%ED%8B%B0%ED%81%B4-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-060-%EC%BD%9C-%EC%8A%A4%ED%83%9DCall-stack%EA%B3%BC-%ED%9E%99Heap

### primitive vs reference
- primitive data type: number / string / boolean / undefined / symbol / null
- reference data type: array / object / function / primitive data type이 아닌 모든 자료형. (heap의 주소만 참조한다.)

[메모리가 저장되는 방식]
- stack : primitive의 값과 fucntion call의 컨텍스트 저장
- heap : object, array, function과 같이 크기가 동적으로 변할 수 있는 reference 값 저장

```js
let p = 1, 
// 주소 p와 값 1은 stack에 저장

let r = [1]
// 주소 r은 stack에 저장, [1]은 heap에 저장

let function f() {
    console.log('heap에 저장')
}
// f는 stack에 저장, function () {}은 stack에 저장
```

- ex code

```js
let x = 2; 
// x 변수에 2라는 값을 stack에 저장
let y = x; 
// y 변수에 x의 값을 stack에 할당. y = 2
y = 3;
// 3을 y의 stack에 저장.

console.log(x) // 2
// x의 값은 변하지 않았으므로 2
```

```js
let x = { foo: 3 }; 
// x라는 변수를 선언하고 heap의 주소를 할당. heap의 주소의 값에는 { foo:3 }이라는 object가 저장
let y = x;
// y라는 변수를 선언하고 x(heap의 주소)를 할당. y라는 변수는 x가 가진 heap의 주소를 참조
y.foo = 2;
// y가 가진 heap의 주소의 값({ foo:3 })을 2로 변경

console.log(x.foo)
// 동시에 참조하는 주소 heap의 값이 변경되었으므로 2
```

```js
'codestates' === 'codestates' // true
3.14 === 3.14 // true

[1,2,3] === [1,2,3] //false
{ foo: 'bar' } === { foo: 'bar' } //false
// // 위 코드는 이미 두 개의 heap 저장 공간의 주소를 확보했습니다.
// 참조 자료형의 `===`는 주소값이 같은지를 확인합니다. 그렇기 때문에 두 참조 자료형의 주소값 은 다르다고 판단을 내릴 수 있습니다.
```