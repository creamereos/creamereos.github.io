---  
layout: post  
title: Prototype Chain JS 21.04.09
subtitle: 
categories: dev
tags: js
comments: true  
--- 

# prototype을 사용한 OOP

- 모든 function은 prototype이라는 속성을 가진다.

- contructor : 생성자. 특정 객체가 생성 될 때 실행 되는 코드

```js
// contructor 함수
let Human = function(name) {
    this.name = name;
}

// Human이란 함수 역시 prototype을 갖는다. 
// Human의 prototype에 method나 속성을 정의 가능하다.
// prototype이 원형 객체(original form)이기 때문에.
// Human > prototype > sleep
Human.prototype.sleep = function() {
    console.log(`${this.name} is sleeping`)
};

// instantiation : instance를 생성하는 과정
let terry = new Human('terry');

// terry.__proto__ === Human.prototype
console.log(terry.__proto__);
// {constructor: ƒ}
//     > constructor: ƒ (name)
//     > __proto__: Object

terry.toString(); // "[object Object]"
// toString()이라는 method를 만든적이 없지만 작동이 된다
// 이유 : 내장 method이기 때문에. 
// terry의 prototype을 .__proto__로 계속 찾다보면 (체이닝)
// 원형 객체(Object)의 prototype에 toString이라는 method를 가지고 있다.

terry.sleep()
// terry is sleeping
```

- prototype chain ex code : 내장 method가 어디에 위치에 있는지 확인하기. 

```js
terry.__proto__ // = Human.prototype

// {sleep: ƒ, constructor: ƒ}
//     sleep: ƒ ()
//     constructor: ƒ (name)
//     __proto__:
//         constructor: ƒ Object()
//         hasOwnProperty: ƒ hasOwnProperty()
//         isPrototypeOf: ƒ isPrototypeOf()
//         propertyIsEnumerable: ƒ propertyIsEnumerable()
//         toLocaleString: ƒ toLocaleString()
//         toString: ƒ toString() // !toString이 여기 위치해있다!
//         valueOf: ƒ valueOf()
//         __defineGetter__: ƒ __defineGetter__()
//         __defineSetter__: ƒ __defineSetter__()
//         __lookupGetter__: ƒ __lookupGetter__()
//         __lookupSetter__: ƒ __lookupSetter__()
//         get __proto__: ƒ __proto__()
//         set __proto__: ƒ __proto__()

terry.__proto__.__proto__
// toString은 terry의 prototype의 prototyep에 있다.
// 즉 toString() method는 terry.__proto__.__proto__ 에 위치해있다.
// 상속의 가장 상위 단계 Object에 있다.
```

- prototype chain ex code 2 : HTML x DOM

```js
let li = document.createElement('li');

li.__proto__ // === HTMLLIElement.prototype

li.__proto__.__proto__ 
// HTMLElement{...}

li.__proto__.__proto__.__proto__ 
// Element{...}

li.__proto__.__proto__.__proto__.__proto__ 
// Node{...}

li.__proto__.__proto__.__proto__.__proto__ .__proto__
// EventTarget {...}

li.__proto__.__proto__.__proto__.__proto__ .__proto__.__proto__
// {consturctor ...}
```

# prototype은 할당 가능 (assignable)

- ex

```js
let makeArr = function() {}

console.log(makeArr.prototype)
// {constructor: ƒ}
//    > constructor: ƒ ()
//    > __proto__: Object


// 자바스크립트에서 함수는 객체 형식.
// 함수 makeArr의 prototype에 Array의 prototype 할당.
// 즉, 함수 makeArr의 prototype에 
// Array의 prototype이 가진 method(push, slice, etc..) 등을 할당
makeArr.prototype = Array.prototyep;

console.log(makeArr.prototype)
// [constructor: ƒ, concat: ƒ, copyWithin: ƒ, fill: ƒ, find: ƒ, …]


let arr = new makeArr();
// 위에서 Array의 prototype을 할당 했으므로 push 등의 method를 사용 가능
arr.push(1) // makeArr [1]
```

### prototype을 사용한 상속 구현

```js
let Human = function(name) {
    this.name = name;
}

Human.prototype.sleep = function() {
    console.log(`${this.name} is sleeping`)
};

let Student = function(name) {
    this.name = name;
}

Student.prototype.learn = function() {
    console.log(`${this.name} is studying programming`)
}

let terry = new Student('terry');

terry.learn(); // terry is studying programming

terry.sleep(); // 오류 발생 : Student가 Human의 sleep method를 상속 받지 못했기 때문에
```

- 상속 방법 

```js
let Student = function(name) {
    Human.call(this.name) = name;
}

// Student의 prototype에 Human의 prototype을 copy하여 그걸 참조한다.
Student.prototype = Object.create(Human.prototype);
// Object.create(객체인자) : 첫번째 객체를 새로운 prototype으로 만든다. 

// 상속을 정확하게 하기 위해서는 연결고리를 다시 연결해줘야한다.
Student.prototype.constructor = Student;

// leran 함수를 할당해준다.
Student.prototype.sleep = function() {
    console.log(`${this.name} is sleeping`)
};


Student.prototype
// {learn: ƒ, sleep: ƒ, constructor: ƒ}

terry.sleep()
// terry is sleeping // 제대로 작동

terry instanceof Student 
// true. 
// a instanceof B : a가 B의 instance인지 확인
```

# prototype으로 상속을 구현하기에는 너무 복잡하다. class keyword를 사용해라.

### class

```js
class Human {
    constructor (name) {
        this.name = name;
    }
    sleep () {
        console.log(`${this.name} is sleeping`)
    }
}

class Student extends Human {
    constructor (name, job) {
        // super를 사용하여 부모의 class의 this까지 상속
        super(name); // 만약 부모 class의 constructor가 똑같다면 생략가능
        // 이 코드에서는 새로운 속성 job을 넣어줬기 때문에 constructor와 super를 사용해야함.
            this.job = job;
    }
    learn () {
        console.log(`${this.name} is studying programming`)
    }

    intro() {
        console.log(`Hi My name is ${this.name}. I'm ${this.job}`)
    }
}

let user1 = new Student('terry', 'developer');

user1 // Student {name: "terry", job: "developer"}

// Human의 method 상속
user1.sleep() // terry is sleeping

// Student의 method
user1.learn() // terry is studying programming
user1.intro() // Hi My name is terry. I'm developer
```

# prototype

```js
function Person (name, job) {
    this.name = name;
    this.job = job;
    this.intro = function() {
        console.log(`Hi my name is ${this.name}, I'm ${job}`)
    }
}

let person1 = new Person ('terry', 'developer')

person1 // Person {name: "terry", job: "developer", intro: ƒ}

person1.toString() // "[object Object]"
// prototyep chain이 작동하는 증거 : toString() method는 person1에 없지만 에러가 발생하지 않는다.
```

위 코드에서 toString() method는 person1에 없지만 에러가 발생하지 않는다. 이는 prototyep chain이 작동하는 증거이다.

person1.toString() 이 코드가 실행될 때 다음과 같은 일이 일어난다.

1. 브라우저는 우선 person1 object가 toString() method를 가지고 있는지 체크

2. 없으므로 person1의 프로토타입 객체(=== Person() 생성자의 프로토타입)에 toString() method가 있는지 체크

3. 여전히 없으므로 Person() 생성자의 프로토타입 객체의 프로토타입 객체(=== Object() 생성자의 프로토타입)가 toString() 메소드를 가지고 있는지 체크. 가지고 있으므로 체이닝 종료.

# 상속되는 속성과 메소드들은 각 객체가 아니라 객체의 constructor(생성자)의 prototype이라는 속성에 정의되어 있다.

```js
Object.prototype
{constructor: ƒ, __defineGetter__: ƒ, __defineSetter__: ƒ, hasOwnProperty: ƒ, __lookupGetter__: ƒ, …}
// Object를 상속받은 객체에서 사용 가능한 수 많은 메소드들이 
// Object의 prototype 속성에 정의.
// prototype도 객체이다. 
```