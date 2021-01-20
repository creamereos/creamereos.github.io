---  
layout: post
title: ES6 - Classes
categories: dev
tags: js
comments: true
---

##### Class 생성 

```js
class Human {
    // constructor는 class를 생성할 때 필요한 데이터를 가지고 있음.
    // class Human의 구조(constructor)는 Human(name, lastname)이다.
    constructor(Name, Age) {
        // this는 해당 class를 참조한다는 뜻
        this.Name = Name;
        this.Age = Age;
    }
}
```

##### Class 사용 (객체로 생성)

```js
const creamer = new Human("creamer", 30);
console.log(creamer);
// Human {Name: "creamer", Age: 30}
```

##### Class 확장 (상속) + @

```js
// intro라는 class는 Human의 모든 데이터, 속성들을 포함하고 있다.
class intro extends Human {
    // 이 부분이 생략되어 포함 된 상태
    // constructor(Name, Age) {
    //     this.Name = Name;
    //     this.Age = Age;

    // extends를 통해 추가적인 함수, 데이터들을 추가 가능하다.
    sayName() {
        console.log(`my name is ${this.Name}`);
    }
    sayAge() {
        console.log(`i'm ${this.Age} years old.`);
    }
}

const Terry = new intro("Jin Terry", 30);
console.log(Terry.sayName(), Terry.sayAge());
// my name is Jin Terry
// i'm 30 years old.
```