---  
layout: post
title: ES6 - classes
categories: dev
tags: js
comments: true
---

- classes : 화려한 Object, 일종의 설계도. (대부분 스스로 만들어 쓰진 않음 보통 라이브러이나 리액트 같은 것을 만들 때 사용)

- class는 일종의 살아있지 않은 설계도 일 뿐이다. 

```js
class CREAMer {
    constructor() {
        this.username = "creamer";
    }
}

console.log(this.username);
// undefined
```

- 설계도를 사용하기 우해서는 new 클래스명();을 사용해서 살아있는 class로 만들어야한다.

```js
class CREAMer {
    constructor() {
        this.username = "creamer";
    }
}

// myUser는 class인 User의 instance(살아있는 Class)다.
const myUser = new CREAMer();

console.log(myUser.username);
// creamer
```

- class x function

```js
class CREAMer {
    constructor() {
        this.username = "creamer";
    }
    sayHello() {
        console.log("Hello");
    }
}

const myUser = new CREAMer();

console.log(myUser.username);
// creamer
setTimeout(myUser.sayHello, 3000);
// Hello
```

- class는 원하는만큼 instance(살아있는 class)를 가질 수 있다.

```js
class Hello {
    sayHello() {
        console.log("Hello");
    }
}

const jsHello = new Hello();
const esHello = new Hello();

jsHello.sayHello();
// Hello
esHello.sayHello();
// Hello
```

- **class constructor argument**

```js
class Hello {
    constructor(name) {
        this.hi = `Hello ${name}` 
    }
}

const jsHello = new Hello("JS World");
const esHello = new Hello("ES World");

console.log(jsHello.hi);
// Hello JS World
console.log(esHello.hi);
// Hello ES World
```

-  class는 object 공장이다. 다수의 object를 가질 수 있다.

```js
class User {
    constructor(name) {
        this.username = name;
    }
    sayHi() {
        console.log(`Hi! ${this.username}`)
    }
}

const user1 = new User("terry");
const user2 = new User("creamer");
const user3 = new User("dreamer");

user1.sayHi();
user2.sayHi();
user3.sayHi();
// Hi! terry
// Hi! creamer
// Hi! dreamer

sayHi();
// Uncaught ReferenceError: sayHi is not defined
// sayHi() function은 오직 class 안에서만 존재하므로 class 없이 사용 불가능하다.
```

```js
class User {
    constructor(name, email, id, pw) {
        this.name = name;
        this.email = email;
        this.id = id;
        this.pw = pw;
        this.word = "I Love my life";
    }
    sayHi() {
        console.log(`Hi, My name is ${this.name}`);
    }
    getInfo() {
        console.log(`${this.name}, ${this.email}, ${this.id}, ${this.pw}`)
    }
    updatePw(newPw, currentPw) {
        if(currentPw === this.pw) {
            this.pw = newPw
        }else {
            console.log("Can't change PW");
        }
    }
}

const creamer = new User("creamer", "creamer@email.com", "creamerid", 12345 );

console.log(creamer.name);
// creamer
console.log(creamer.email);
// creamer@email.com
console.log(creamer.id);
// creamerid
console.log(creamer.pw);
// 12345
creamer.updatePw(54312, 123);
console.log(creamer.pw);
// Can't change PW

creamer.updatePw(54312, 12345);
console.log(creamer.pw);
// 54321


console.log(creamer.word);
// I Love my life

console.log(creamer.getInfo());
// creamer, creamer@email.com, creamerid, 54321
```

- class extend : class의 확장. 다른 class의 기능 및 정보(property, function 등)를 가지고와서 적용하고 추가적인 기능 및 정보를 추가 할 수 있다.

```js
// class인 Admin은 User라는 class의 기능들을 가져온다.
class Admin extends User {
    deleteWebsite() {
        console.log("Deleting the whole website...");
    }
}

const admin = new Admin("admin", "admin@email.com", "adminid", 67890);

// 기존 class인 User의 property들을 가지고 올 수 있다.
console.log(admin.getInfo())
// admin, admin@email.com, adminid, 67890

admin.deleteWebsite();
// Deleting the whole website...
```

- class를 확장하는 경우 확장하는 class에는 consructor()를 설정해주면 안된다. 확장하는 class에 cuntructor를 지정할 경우 가져오는 기존의 class constructor()가 초기화(overwrite) 된다.

```js
// 연속적으로 확장 가능하다. (ex. User => Admin => Master)
class Master extends Admin {
    constructor(superadmin) {
        this.superadmin = superadmin;
    }
}

const mater = new Master("master", "master@email.com", "masterid", 67890)
console.log(master.getInfo());
// VM5192:3 Uncaught ReferenceError: 
// Must call super constructor in derived class before accessing 'this' 
// or returning from derived constructor
```

- super(); : extends를 하면서 새로운 contructor()를 사용 할 때 발생하는 문제를 해결하는 방법. 

새로운 constructor(){}를 적용하고 그 안에 super();를 넣어 준다.
super();를 사용하면 기존 class의 contructor()를 호출한다.

```js
class User {
    // class constructor의 property들이 너무 많아지면 object 형식으로 변경 가능
    constructor({name, email, id, pw}){
        this.name = name;
        this.email = email;
        this.id = id;
        this.pw = pw;
    }
}

const creamer = new User({
    name: "cremaer",
    email: "creamer@email.com",
    id: "creamerid",
    pw: 12345
});

class Admin extends User {
    constructor({name, email, id, pw, superAdmin }) {
        // 기존 class의 property들을 호출
        super({name, email, id, pw});
        this.superAdmin = superAdmin;
    }
   
}

const admin = new Admin({
    name: "admin",
    email: "admin@email.com",
    id: "adminid",
    pw: 12345,
    superAdmin: true
})

console.log(admin.name, admin.email, admin.id, admin.pw, admin.superAdmin);
// admin admin@email.com adminid 12345 true

// 확장 된 class인 Admin의 새로운 property 뿐만 아니라 
// 기존 class User의 property들 또한 불러 올 수 있다.
```
