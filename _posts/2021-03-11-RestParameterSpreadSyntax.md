---  
layout: post  
title: JS 21.03.11
subtitle: Spread Syntax vs Rest Parameter
categories: dev
tags: js
comments: true  
--- 

### Spread Syntax(분리) vs Rest Parameter(배열로 합침)

##### Spread Operator & Syntax : 
배열, 문자열, 객체 등 반복 가능한 객체(Iterable Object)를 개별 요소로 분리. (**연결, 복사** 등에 사용)

```js
...배열 : string, array, object로 가능
let arr = [1,2,3,4,5]
console.log(...arr)
// 1, 2, 3, 4, 5

console.log([...arr]) // 분리한 후 다시 array에 담아주기
// [1, 2, 3, 4, 5]

console.log({...arr}) // 분리한 후 object에 담아주기 (index : value 형태로 저장)
// {0: 1, 1: 2, 2: 3, 3: 4, 4: 5}

...문자열 : string, array, object로 가능
let str = 'abcdefghi'
console.log(...str)
// a b c d e f g h i

console.log([...str]) // 분리한 후 array에 담아주기
// ["a", "b", "c", "d", "e", "f", "g", "h", "i"]

console.log({...str}) // 분리한 후 object에 담아주기 (index : value 형태로 저장)
// {0: "a", 1: "b", 2: "c", 3: "d", 4: "e", 5: "f", 6: "g", 7: "h", 8: "i"}

...객체  : object만 가능
let obj = { a: 0, b: 1}
console.log({...obj}) // key-value는 {} 안에 있어야함
// { a: 0, b: 1} 
```

- ex code

```js
// 01
function sum(x, y, z) { 
    return x + y + z; 
    } 
const numbers = [1, 2, 3]; 
console.log(sum(...numbers)); // 6

// 02
const numbers = [1, 2, 3];
const num = [...numbers, 4, 5]
// [1, 2, 3, 4, 5]

// 03
const foo = { x: 1}; 
const bar = { y: 2}; 
const foobar = {...foo, ...bar}; // {x: 1 , y: 2}

// ...을 사용하면 복사된다.
num === [...num] // false
obj === {...obj} // false
```

##### Rest Parameter :
Spread 연산자(...)를 사용하여 function의 Parameter를 작성한 형태. 즉, **Rest Parameter를 사용하면 function의 Parameter로 오는 값**들을 **Array**로 전달 받는다.

- syntax

```js
function 함수명(...Rest Parameter) {
  실행문
}

// RestParameter 앞에 일반적인 파라미터를 받을 수도 있다. 
// RestParameter는 반드시 마지막 Parameter로 있어야한다.

function 함수명(파라미터1, 파라미터2, ...Rest Parameter) {
  실행문
}
```

- ex code

```js
// 1
function restP(a, b, ...manyMoreArgs) {
  console.log("a", a);
  console.log("b", b);
  console.log("manyMoreArgs", manyMoreArgs);
}

myFun("one", "two", "three", "four", "five", "six");
// a, one
// b, two
// manyMoreArgs, [three, four, five, six]
// rest parameter는 배열 형태로.

// 2
function sum(...nums) {
      let sum = 0;
      for (let i = 0; i < nums.length; i++) {
        sum = sum + nums[i];
      }
      return sum;
}
sum(1, 2, 3) // 6)
sum(1, 2, 3, 4) // 10

// 3

function getAllParams(required1, required2, ...args) {
      return [required1, required2, args];
}
getAllParams(123)
// [123, undefined, []]

// 4
function makePizza(dough, name, ...toppings) {
      const order = `You ordered ${name} pizza with ${dough} dough and ${toppings.length} extra toppings!`;
      return order;
    }
    makePizza('original')
    // 'You ordered undefined pizza with original dough and 0 extra toppings!'
    makePizza('thin', 'pepperoni')
    // 'You ordered pepperoni pizza with thin dough and 0 extra toppings!'
    makePizza('napoli', 'meat', 'extra cheese', 'onion', 'bacon')
    'You ordered meat pizza with napoli dough and 3 extra toppings!'
```
