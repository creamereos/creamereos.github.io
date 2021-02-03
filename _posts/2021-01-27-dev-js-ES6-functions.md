---  
layout: post
title: ES6 - functions
categories: dev
tags: js
comments: true
---

### arrow function

- ES6 이전 function

```js
const number = [1,2,3,4,5]

const numbers = number.map(function(argument){
    return argument + '번';
});

console.log(numbers);
// ["1번", "2번", "3번", "4번", "5번"]
```

- ES6 arrow function 적용 
- arrow function => 을 사용할 때 바로 {}가 나오면 안된다.

```js
const number = [1,2,3,4,5]

// return 해야 할 expression표현식;이 한줄이면 return과 중괄호{}를 생략해야한다.
const numbers = number.map(argument => argument + '번');

console.log(numbers);
// ["1번", "2번", "3번", "4번", "5번"]
```

### this in arrow function

- arrow function에서 this를 사용하면 this는 window를 가리킨다.

```js
const CREAMer = {
    name: "Terry",
    age: 30,
    // 실행되지 않는 함수. 
    addYear: () => this.age++
    // 여기서 this는 window를 가리킨다.
};

console.log(CREAMer);
// {name: "Terry", age: 30, addYear: ƒ}
CREAMer.addYear();
CREAMer.addYear();
console.log(CREAMer);
// {name: "Terry", age: 30, addYear: ƒ}
```

- this를 사용하고 싶으면 arrow function을 사용하지마라.

```js
const CREAMer = {
    name: "Terry",
    age: 30,
    addYear(){
        this.age++;
    }
};

console.log(CREAMer);
// {name: "Terry", age: 30, addYear: ƒ}
CREAMer.addYear();
CREAMer.addYear();
console.log(CREAMer);
// {name: "Terry", age: 32, addYear: ƒ}
```

### arrow function in real world

- array.find() : 조건을 만족하는 첫번째 element를 return.

```js
const emails = [
        "abc@naver.com",
        "def@gmail.com",
        "ghi@gmail.com",
        "jkl@naver.com",
        "mnp@creamer.com"
];

// includes(something)는 something이 포함되어있는지 체크
const foundEmail = emails.find(argument => argument.includes("@gmail.com"));

console.log(foundEmail);
// def@gmail.com

const number = [5, 12, 8, 130, 44];

const foundNumber = number.find(argument => argument > 10);

console.log(found);
// 12
```

- array.filter() : 조건을 통과한 elements로 새로운 array를 생성.

```js
const emails = [
        "abc@naver.com",
        "def@gmail.com",
        "ghi@gmail.com",
        "jkl@naver.com",
        "mnp@creamer.com"
];

const noGmail = emails.filter(argument => !argument.includes("@gmail.com"));

console.log(noGmail);
// ["abc@naver.com", "jkl@naver.com", "mnp@creamer.com"]
```

- forEach() : array의 각 element 마다 제공된 function 실행 (새로운 array를 만들지는 않음)

```js
const emails = [
        "abc@naver.com",
        "def@gmail.com",
        "ghi@gmail.com",
        "jkl@naver.com",
        "mnp@creamer.com"
];

emails.forEach(argument => {
    console.log(argument.split("@")[0]);
})

// abc 
// def 
// ghi 
// jkl 
// mnp 
```

- map() : array의 각 element 마다 제공된 function 실행하고 새로운 array 생성

```js
const emails = [
        "abc@naver.com",
        "def@gmail.com",
        "ghi@gmail.com",
        "jkl@naver.com",
        "mnp@creamer.com"
];

const emailName = emails.map(argument => argument.split("@")[0])

console.log(emailName);
// ["abc", "def", "ghi", "jkl", "mnp"]
```

- Object Return

```js
const emails = [
        "abc@naver.com",
        "def@gmail.com",
        "ghi@gmail.com",
        "jkl@naver.com",
        "mnp@creamer.com"
];

// implicit return => 을 사용하기 위해서는 바로 {}가 나오면 안된다. 때문에 ()로 감싸준다.
const emailObj = emails.map((argument,index) => ({username: argument.split("@")[0], points: index}));

console.table(emailObj);
```

# implicit return => 을 사용하기 위해서는 바로 {}가 나오면 안된다. object를 return하기 위해서는 ()로 감싸준다.


### Default Values

```js
function sayHi(aName) {
    return "Hi " + aName;
}

console.log(sayHi("Terry"));
// Hi Terry 

console.log(sayHi());
// Hi undefined 
```

- default values 설정하기 (string, function, object 등을 default values로 설정 가능)

```js
function sayHi(aName = "Stranger") {
    return "Hi " + aName;
}

console.log(sayHi());
// Hi Stranger 
```

- default value를 arrow function으로 변경하기

```js
const sayHi = (aName = "Stranger") => "Hi " + aName;

console.log(sayHi());
// Hi Stranger 
```