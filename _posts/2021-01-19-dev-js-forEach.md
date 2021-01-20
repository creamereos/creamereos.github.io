---  
layout: post
title: JS - Array forEach / Push / include
categories: dev
tags: js
comments: true
---

- forEach() : 기존 array의 각 argument에 함수를 실행. (map은 함수를 실행하고 새로운 array를 **return**)

```js
let numbers = [0, 1, 2, 3, 4];
numbers.forEach(number => console.log(number + 1));
// 1
// 2
// 3
// 4
// 5
```

- push() : 기존 array에 새로운 element 추가

```js
numbers.push(5);
console.log(numbers);
// [0, 1, 2, 3, 4, 5]
```

- includes() : array에 해당 element가 있는 지 확인

```js
if(!numbers.includes(6)) {
    numbers.push(6);
}
console.log(numbers);
// [0, 1, 2, 3, 4, 5, 6]
```

- revers() : array를 역순으로 변경

```js
console.log(numbers.reverse());
// [6, 5, 4, 3, 2, 1, 0]
```