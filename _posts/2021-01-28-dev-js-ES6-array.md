---  
layout: post
title: ES6 - Array
categories: dev
tags: js
comments: true
---

- Array.of() : Array 생성

```js
const numbers = Array.of(1, 2, 3, 4, 5);
console.log(numbers);
// [1, 2, 3, 4, 5]
```

- Array.from() : 유사 배열 객체(array-like object)나 반복 가능한 객체(iterable object)를 얕게 복사해 새로운 Array 객체 생성

```js
// string -> array
Array.from('foo');
// ["f", "o", "o"]

// array-like object -> array
function f() {
  return Array.from(arguments);
}

f(1, 2, 3);
// [1, 2, 3]
```

- Array.find() : 함수를 만족하는 첫 번째 요소의 값을 반환. 요소가 없다면 undefined를 반환.

```js
const numbers = [1, 10, 100, 1000];

const checkNumbers = numbers.find(number => number > 99);

console.log(checkNumbers);
// 100
```

- Array.findIndex() : 함수를 만족하는 배열의 첫 번째 요소에 대한 인덱스를 반환. 만족하는 요소가 없으면 -1을 반환.

```js
const numbers = [1, 10, 100, 1000];

const checkNumbers = numbers.findIndex(number => number > 99);

console.log(checkNumbers);
// 2
```

- Array.fill() : Array의 start index부터 end index의 이전까지 정적인 값 하나로 채운다.

```js
const num = [1, 2, 3, 4];

// fill with 0 from position 2 until position 4
console.log(num.fill(0, 2, 4));
// expected output: [1, 2, 0, 0]

// fill with 5 from position 1
console.log(num.fill(5, 1));
// expected output: [1, 5, 5, 5]

console.log(num.fill(6));
// expected output: [6, 6, 6, 6]
```

- Array.includes() : 포함하고 있는지 체크하여 true / false로 반환

```js
const num = [1,2,3,4,5];
console.log(num.includes(3));
// true
console.log(num.includes(6));
// false
```
