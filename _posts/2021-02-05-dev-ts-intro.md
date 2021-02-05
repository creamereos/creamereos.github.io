---  
layout: post
title: TS intro (setting type / interface)
categories: dev
tags: ts
comments: true
---

- Type Script : Java Script를 보완한 언어(실제로는 JS 언어를 사용하는 것). **Type(String, Number, Object, Boolean, etc..)을 미리 정의**해주어 잘못된 코드가 실행되는 실수 감소

- function arguments type

```js
const plus = (a, b) => a + b;

console.log(plus(2, "string"));
// string2
```

- 반면 ts는 미리 Type을 정해주어 코드 실행 전 오류를 찾을 수 있게 해준다.

```ts
const plus = (a:number, b:number) => a + b;

console.log(plus(2, "string"));
// 에러 발생. b의 타입을 number로 지정해 주었기 때문이 실행 되지 않는다.
```

- variable type

```ts
let hello:string = "hello";

hello = "4";
```

- return type

```ts
const greet = (name:string, age:number):string => {
  return `Hello ${name}! you are ${age} years old`
}

console.log(greet("CREAMer", 30));
// Hello CREAMer! you are 30 years old
```

- name은 string이므로 오류가 발생하지 않는다.

```ts
const greet = (name:string, age:number):string => {
  return name
}

console.log(greet("CREAMer", 30));
// CREAMer
```

- age는 number이므로 오류가 발생한다.

```ts
const greet = (name:string, age:number):string => {
  return age
}

console.log(greet("CREAMer", 30));
// error TS2322: Type 'number' is not assignable to type 'string'.
```

- 에러 발생 : return 값이 string이 아닌 console.log 이므로

```ts
const greet = (name:string, age:number):string => {
  console.log(`Hello ${name}! you are ${age} years old`)
}

console.log(greet("CREAMer", 30));
// A function whose declared type is neither 'void' nor 'any' must return a value.
```

# Type Script Interface

- js : js에서는 아래 코드가 실행된다. helloToHuman이란 function의 argumnet인 human을 따로 정의해주지 않아도 자바스크립트에서는 최대한 실행시킨다.

```js
const creamer = {
  name: 'terry',
  age: 30,
  happy: true
}

const helloToHuman = (human) => {
  console.log(`Hello ${human.name}! you are ${human.age} old`);
}

helloToHuman(creamer);
// Hello terry! you are 30 old
```

- ts : onject variable인 creamer의 각 property의 type이 정해져 있지 않았고 creamer를 helloToHuman의 parameter(=argumnet)인 humman에 넣었을 때 타입이 정해져 있지 않아 오류가 발생한다.

```ts
const creamer = {
  name: 'terry',
  age: 30,
  happy: true
}

const helloToHuman = (human) => {
  console.log(`Hello ${human.name}! you are ${human.age} old`);
}

helloToHuman(creamer);
// 오류 발생 : Parameter 'creamer' implicitly has an 'any' type.
```

- ts에서 해결 방법 : **interface**를 사용하여 object의 각 property의 type을 정해주고 function의 argument의  type을 interface로 지정해준다.

```ts
const creamer = {
  name: 'terry',
  age: 30,
  happy: true
}

interface interHuman{
    name: string;
    age: number;
    happy: boolean;
}

const helloToHuman = (human: interHuman) => {
  console.log(`Hello ${human.name}! you are ${human.age} old`);
}

helloToHuman(creamer);
// Hello terry! you are 30 old
```

- 특정 property의 value를 비어있는 경우 오류 발생

```ts
const creamer = {
  name: 'terry',
  age: 30,
}

interface interHuman{
    name: string;
    age: number;
    happy: boolean;
}

const helloToHuman = (human: interHuman) => {
  console.log(`Hello ${human.name}! you are ${human.age} old`);
}

helloToHuman(creamer);
// property인 happy의 value가 없으므로 오류 발생
```

- 해결 방법 : TS optional value setting : 물음표를 붙여준다.

```ts
const creamer = {
  name: 'terry',
  age: 30,
}

interface interHuman{
    name: string;
    age: number;
    happy?: boolean;
    // 비어있는 property에 물음표 추가
    // 의미는 해당 property는 undefine 또는 boolean이다.
}

const helloToHuman = (human: interHuman) => {
  console.log(`Hello ${human.name}! you are ${human.age} old. are you ${human.happy}?`);
}

helloToHuman(creamer);
// Hello terry! you are 30 old. are you undefined?
// happy의 값이 비어있으므로 undefined 
```