---  
layout: post
title: ES6 - For of Loop
categories: dev
tags: js
comments: true
---

- for

```js
const numbers = [1,2,3,4,5,6,7,8,9,0];

for(let i=0; i < numbers.length; i++){
    console.log(numbers[i]);
}

// 1
// 2
// 3
// 4
// 5
// 6
// 7
// 8
// 9
// 0
```

- forEach : array의 각 element 마다 function 실행

```js
const numbers = [1,2,3,4,5,6,7,8,9,0];
numbers.forEach(number => console.log(number));
// 1
// 2
// 3
// 4
// 5
// 6
// 7
// 8
// 9
// 0
```

- for of

```js
const numbers = [1,2,3,4,5,6,7,8,9,0];

for (const number of numbers) {
    console.log(number);
}
// 1
// 2
// 3
// 4
// 5
// 6
// 7
// 8
// 9
// 0

// forEach와는 다르게 let으로 변수 설정 가능. forE
for (let number of numbers) {
    console.log(number);
}

// forEach와는 다르게 string에서도 사용 가능. (array, lterables, NodeList 등도 사용 가능)
for (const letter of "helllloooooooo this is very loooooooooong") {
    console.log(letter);
}

// loop 중에 특정 단어를 찾았을 때 멈추기 가능
for (const number of numbers) {
    if(number === 6){
        break;
    }
    console.log(number);
}
// 1
// 2
// 3
// 4
// 5
```