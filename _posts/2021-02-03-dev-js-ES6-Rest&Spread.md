---  
layout: post
title: ES6 - Rest & Spread
categories: dev
tags: js
comments: true
---

- array spread : 변수를 array 밖으로 가져와서 나열

```js
const numbers1 = [1,2,3,4,5];

console.log(numbers1);
// [1,2,3,4,5]
console.log(...numbers1);
// 1,2,3,4,5

const numbers2 = [6,7,8,9,0];

console.log(numbers1 + numbers2);
// 1,2,3,4,56,7,8,9,0 

console.log([numbers1, numbers2]);
// [Array(5), Array(5)]

console.log(...numbers1, ...numbers2];
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

console.log([...numbers1, ...numbers2]);
// [1, 2, 3, 4, 5, 6, 7, 8, 9, 0]
```

- object spread : 변수를 object 밖으로 가져와서 나열

```js
const creamer1 = {
    name: "creamer",
    job: "blockcahin"
};

const creamer2 = {
    age : 30,
    live: "Earth"
};

// ...로 해체 후 하나의 {}에 담아주기
console.log({ ...creamer1, ...creamer2 });
// {name: "creamer", job: "blockcahin", age: 30, live: "Earth"}
```

- array spread +

```js
const numbers = [1,2,3,4,5]

const newNumbers = [0, ...numbers, 6];

console.log(newNumbers);
// [0, 1, 2, 3, 4, 5, 6]
```

- object spread + 

```js
const creamer = {
    username: "creamer7"
};

console.log({...creamer, pw: 123});
// {username: "creamer7", pw: 123}
```

- array merge

```js
const number1 = [1, 2, 3];

const number2 = [6, 7];

const numbers = [...number1, 4, 5, ...number2];

console.log(numbers);
// [1, 2, 3, 4, 5, 6, 7]
```

- condition object

```js
const lastName = prompt("Last name");

const user = {
  username: "creamer",  
  age: 30,
  //  조건 ? true면 실행 : false면 실행
  //  lastName: lastName !== "" ? lastName : undefined
  // 위 코드를 아래와 같이 변경 가능
  ...(lastName !== "" && {lastName})
  // ...(lastName !== "" && lastName: lastName)
  // ...(A && B) A와 B 두가지 조건이 모두 true여야만 한다.
  // ... 3개의 점이 조건의 결과를 Spread
  
};

console.log(user);
// prompt에 lee 입력 시
// {username: "creamer", age: 30, lastName: "lee"}

// prompt에 아무것도 입력 하지 않았을 때
// {username: "creamer", age: 30}
```

- rest Parameters 

rest는 모든 값을 하나의 변수로 축소 시켜준다.

```js
// ...이 rest 문법
const infiniteArgs = (...Rest) => console.log(Rest);

infiniteArgs("1", 2, true, "lala", [1,3,4,5]);
// ["1", 2, true, "lala", Array(4)]
```

```js
const bestNumbers = (here, ...Rest) => { 
    console.log(`My best Number is ${here}`);
    console.log(Rest)
}

bestNumbers(1,2,3,4,5,6,7);
// My best Number is 1 
// (6) [2, 3, 4, 5, 6, 7]

bestNumbers(7,6,5,4,3,2,1);
// My best Number is 7 
// (6) [6, 5, 4, 3, 2, 1]
```

- destructuring + rest

```js
const user = {
    name : "terry",
    age: 30,
    pw: 123
};

// user["pw"] = null;

// destructuring과 rest 동시에 사용하기
const killPw = ({pw, ...rest}) => rest;

const userInfo = killPw(user);

console.log(userInfo);
// {name: "terry", age: 30}
```

- rest + destructuring 

```js
const user = {
    name : "terry",
    age: 30,
    pw: 123
};

// destructuring으로 country라는 property를 추가해주고 korea를 deafult 값으로 넣어준다.
// rest로 country를 제외한 나머지 property들을 축소해서 모아주고 return
const setCountry = ({country = "Korea", ...rest}) => ({country, ...rest});

const userInfo = setCountry(user);

console.log(userInfo);
// {country: "Korea", name: "terry", age: 30, pw: 123}
```

- rename + destructuring + rest + spread

```js
const user = {
    a1b2c3 : "terry",
    age: 30,
    pw: 123
};

console.log(user);
// {a1b2c3: "terry", age: 30, pw: 123}

// 앞에 ...rest는 rest, 뒤에 ...rest는 spread. 
const rename = ({ a1b2c3: name, ...rest }) => ({name, ...rest});

console.log(rename(user));
// {name: "terry", age: 30, pw: 123}
```