---  
layout: post
title: JS - Array Map & Filter
categories: dev
tags: js
comments: true
---

##### Array Map

map()은 callbackFunction을 실행한 결과를 가지고 새로운 배열을 만들 때 사용한다.

- syntax

```js
array.map(callbackFunction(currenValue, index, array), thisArg)
```

- argument인 arg는 array의 각 item을 가리킨다.

```js
const days = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"];

const day = days.map(arg => console.log(arg));
// Mon
// Tue
// Wed
// Thu
// Fri
// Sat
// Sun
```

- 각 item에 function을 실행한 값으로 새로운 array를 생성

```js
const happyDays = days.map(arg => `🥰 ${A}`);
// ["🥰 Mon", "🥰 Tue", "🥰 Wed", "🥰 Thu", "🥰 Fri", "🥰 Sat", "🥰 Sun"]
```

- map() 안의 함수를 밖에서 사용할 수 도 있다.

```js
const addSad = A => `😭 ${A}`;
const sadDays = days.map(addSad);
console.log(sadDays);
// ["😭 Mon", "😭 Tue", "😭 Wed", "😭 Thu", "😭 Fri", "😭 Sat", "😭 Sun"]
```

- map의 2번째 argument를 넣으면 index를 표시

```js
const numberDays = days.map((arg, number) => `${number} ${arg}`)
console.log(numberDays);
// ["0 Mon", "1 Tue", "2 Wed", "3 Thu", "4 Fri", "5 Sat", "6 Sun"]
```

##### Array Filter

- Filter : array의 모든 요소들에 Function을 실행해서 ture를 return하는 요소들로만 array에 포함

```js
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

const evenNumbers = numbers.filter(number => number % 2 === 0);

console.log(evenNumbers);
// [2, 4, 6, 8, 10]
```

- 조건이 되는 function을 외부에서도 사용 가능

```js
const oddCondition = number => number % 2 !== 0;

const oddNumbers = numbers.filter(oddCondition);

console.log(oddNumbers);
// [1, 3, 5, 7, 9]
```

- let 사용하여 const 사용 안하기

```js
let alphabet = ['a', 'b', 'c', 'ㄱ'];
alphabet = alphabet.filter(eng => eng !== "ㄱ");
console.log(alphabet);
// ["a", "b", "c"]
```