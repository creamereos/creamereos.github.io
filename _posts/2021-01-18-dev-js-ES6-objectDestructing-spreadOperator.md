---  
layout: post
title: Object Destructing / Spread Operator
categories: dev
tags: js
comments: true
---

##### Object Destructuring

```js
// object를 담고 있는 variable 
const human = {
    name: "creamer",
    age: 30,
    nationality: "korean",
    // object 안에 object
    favFood : {
        breakfast: "Vegetable Juice",
        lunch: "Curry",
        dinner: "Fish"
    }
}
```

위 object variable을 사용하여 데이터 추출

```js
const name = human.name;
const age = human.age;
const national = human.nationality;
const favFood = human.favFood;

console.log(name, age, national, favFood);
```

위 코드는 비슷한 코드를 여러번 작성하기 때문에 효율적이지 않다. 

- Object를 Destructing 하기

```js
const { 
    name, 
    age, 
    // variable를 다른 이름으로 설정 가능
    nationality: difName, 
    // {}를 추가하면 variable 안에서 {}안에 있는 단어를 찾는다는 걸 의미
    favFood: {dinner, breakfast, lunch} 
} = human;

console.log(name, age, difName, dinner, breakfast, lunch);
// creamer 30 korean Fish Vegetable Juice Curry
```

##### Spread Operator

```js
const weekday = ["Mon", "Tue", "Wed", "Thu", "Fri,"];
const weekend = ["Sat", "Sun"];
```

array의 합은 string으로 변경

```js
let allDays = weekday + weekend;

console.log(allDays);
// Mon,Tue,Wed,Thu,FriSat,Sun
```

string을 array 안에 담기

```js
let allDays = [weekday + weekend];
console.log(allDays);
// [Mon,Tue,Wed,Thu,FriSat,Sun]
```

array로 가져오기

```js
let allDays = [weekday, weekend];
console.log(allDays);
// (2) [Array(5), Array(2)]
// 0: (5) ["Mon", "Tue", "Wed", "Thu", "Fri,"]
// 1: (2) ["Sat", "Sun"]
// length: 2
// __proto__: Array(0)
```

- Spread Opreator :각 array의 item을 가져와 unpack (... 3개 붙이기)
(두 array의 item들이 한 array에 있게 됨)

```js
let allDays = [...weekday, ...weekend, "Every Day Happy Day"];
console.log(allDays);
// ["Mon", "Tue", "Wed", "Thu", "Fri,", "Sat", "Sun", "Every Day Happy Day"]
```

- object에서 spread operator 사용

```js
const ob1 = {
    first: "hi",
    second: "hello"
}

const ob2 = {
    third: "bye"
}

const OB = {...ob1, ...ob2};
console.log(OB);
// {first: "hi", second: "hello", third: "bye"}
```

- function에 spread operator 사용

```js
...
const arrFun = (something, args) => console.log(...args);
```