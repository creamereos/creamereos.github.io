---  
layout: post  
title: prototype & class JS 21.04.12
subtitle: 
categories: dev
tags: js
comments: true  
--- 

# __proto__, constructor,  prototype 관계

![](/assets/img/post/2021-04-09-15-27-18.png)

```js
let Human = function (name, job) {
    this.name = name;
    this.job = job;
}

Human.prototype.intro = function () {
    console.log(`Hi my name is ${this.name}, I'm ${job}`)
}

let creamer  = new Human('terry', 'developer');
```

- Human의 prototype에는 constructor(생성자) 함수와 intro 함수가 있다.

```js
Human.prototype
// {intro: ƒ, constructor: ƒ}
```

- Human으로 생성한 instance는 creamer이다.

- creamer의 .__proto__는 Human의 prototype이다.

(.__proto__를 참조하는 부모라고 생각하면 이해하기 쉬운것 같기도..)

```js
creamer.__proto__  === Human.prototype
// true
```

# class & super

### class 선언 방법

```js
// 선언식 : A라는 이름을 가진 class를 선언
class A {
    constructor() {

    }
}

// 표현식 1(class 이름 미설정): class를 A라는 변수에 담아준다.
let A = class {
    constructor
}

// 표현식 2(class 이름 설정) : B라는 class를 A라는 변수에 담아준다.
let A = class B {
    constructor
}
// 이름을 가진 class 표현식의 이름(===B)은 클래스 body의 local scope에 한해 유
```

### class body

- {} 사이의 안쪽 부분. 
- constructor method와 새로운 method 정의. 
- stric mode에서 실행 된다.

##### constructor method

- **class로 생성된 객체를 생성하고 초기화**하기 위한 특수한 method. 
- constructor 라는 이름을 가진 method는 **class안에 오직 하나만 존재** 가능
(여러개가 존재하면 SyntaxError 발생)
- **상속 시 부모 클래스의 constructor를 호출**하기 위해 **super 키워드** 사용 가능

```js
class Person {
    // constructor method
    constructor(name, job) {
        this.name = name;
        this.job = job
    }
    // method 함수 추가
    intro() {
        console.log(`Hi my name is ${name}. I'm ${job}`)
    }
}

// 새로운 객체 생성 
let person1 = new Person('terry', 'developer')
```

### sub classing (클래스 상속)

- extends 키워드를 사용하여 부모 클래스의 constructor와 method를 상속 받을 수 있다.

syntax

```js
class 자식클래스 extends 부모클래스 {
    // 상속받은 부모클래스의 constructor, method, 등 (생략 가능)

    추가할 method, construcor 속성 
}
```

```js
class User {
    constructor(name, mail, id, pw) {
        this.name = name;
        this.mail = mail;
        this.id = id;
        this.pw = pw;
        this.word = '사용자 정보'
    }
    getInfo() {
        console.log(`${this.name} ${this.mail} ${this.id} ${this.pw}`)
    }

    changePw(curPw, newPw) {
        if (curPw === this.pw) {
            this.pw = newPw;
            alert('비밀번호가 정상적으로 변경되었습니다.')
        } else {
            alert('현재 비밀번호가 일치하지 않습니다.')
        }
    }
}

let user1 = new User('terry', 'terry@gmail.co.kr', 'terry7', 12345);

user1 
//{ name: "terry", mail: "terry@gmail.co.kr", id: "terry7", pw: "12345", word: "사용자 정보"}

user1.getInfo() 
// terry terry@gmail.co.kr terry7 12345

user1.changePw(123, 54321)
// 현재 비밀번호가 일치하지 않습니다.

user1.changePw(12345, 54321)
// 비밀번호가 정상적으로 변경되었습니다.

user1.getInfo()
// terry terry@gmail.co.kr terry7 54321
```

### Class 상속

- 예시 01 : 상속 후 method 추가

```js
class Admin extends User {
    // 상속 받은 constructor 코드가 생략된 상태

    // constructor(name, mail, id, pw) {
    //     this.name = name;
    //     this.mail = mail;
    //     this.id = id;
    //     this.pw = pw;
    //     this.word = '사용자 정보'
    // }

    // 상속 받은 method도 생략된 상태

    // getInfo() {
    //     console.log(`${this.name} ${this.mail} ${this.id} ${this.pw}`)
    // }

    // changePw(curPw, newPw) {
    //     if (curPw === this.pw) {
    //         this.pw = newPw;
    //         alert('비밀번호가 정상적으로 변경되었습니다.')
    //     } else {
    //         alert('현재 비밀번호가 일치하지 않습니다.')
    //     }
    // }

    deleteWebsite() {
        console.log("홈페이지 삭제 중...")
    }
}

let webAdmin = new Admin('admin', 'admin@google.co.kr', 'admin1', 123);

// 상속 받은 constructor
webAdmin
// Admin {name: "admin", mail: "admin@google.co.kr", id: "admin1", pw: 123, word: "사용자 정보"}

// 상속 받은 method
webAdmin.getInfo()
// admin admin@google.co.kr admin1 123

// 별도로 추가한 method
webAdmin.deleteWebsite()
// 홈페이지 삭제 중...
```

### class 상속의 규칙 

- class를 extends 하는 경우 상속 받는 class에는 contructor를 성정해주면 안된다. 상속받는 class에 constructor를 지정할 경우 부모 class로 부터 상속받는 constructor가 초기화(overwirte)된다.

- 잘못된 예시

```js
class superAdmin extends Admin {
    constructor (superAdmin) {
        this.superAdmin = superAdmin;
    }
}

let sAdmin = new superAdmin ('SUPER ADMIN', 'superA@mail.com', 'supera', '1');
// 에러 발생 : 
// Uncaught ReferenceError: 
// Must call super constructor in derived class before accessing 'this' 
// or returning from derived constructor
```

에러가 발생하는 원인은 Class superAdmin이 contructor를 재선언하여 기존 User의 constructor에 overwrite.
때문에 기존 contructor 구조로 객체를 생성 불가능하다. 

**이 문제를 해결하기 위해서는 super()를 사용**해야한다.

### super() : extends를 하면서 새로운 contructor()를 사용 할 때 발생하는 문제를 해결하는 방법.

- syntax

```js
class 자식클래스 extends 부모클래스 {
    constructor(기존 constructor에서 가져올 속성 + 추가할 속성) {
        super(기존 constructor에서 가져올 속성)
        // this를 사용해 새로 추가할 속성 보다 super가 반드시 먼저 호출되야한다.
        this.기존 constructor 외 추가할 키 = 값
    }
}
```

```js
class SuperAdmin extends Admin {
    constructor(name, email, id, pw, superPower) {

        super(name, email, id, pw);

        this.superPower = this.superPower;
    }

    allDelete() {
        console.log('웹사이트와 모든 유저 정보를 삭제 중...')
    }
}

let sAdmin = new SuperAdmin("god", "godview@google.co.kr", "god7", 12345, 'SUPER POWER')

sAdmin
// SuperAdmin {name: "god", mail: "godview@google.co.kr", id: "god7", pw: 12345, word: "사용자 정보", …}

sAdmin.getInfo()
// god godview@google.co.kr god7 12345

sAdmin.allDelete() 
// 웹사이트와 모든 유저 정보를 삭제 중...
```