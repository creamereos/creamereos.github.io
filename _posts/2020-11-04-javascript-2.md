---  
layout: post
title: Node JS - class, promise,
subtitle:
categories: dev
tags: javascript
comments: true  
--- 

# class
클래스 문법으로 바뀌었더라도 JS는 프로토타입 기반으로 동작.

~~~
class Human {
  constuctor(type='human'){
    this.type = type;
  }
}

  static isHuman(human) {
    return human instanceof Human;
  }

  breath() {
    alert('h-a-a-a-m');
  }

  class Zero extends Human {
    constructor(type, firstName, lastName){
      super(type);
      this.firstName = firstName;
      this.lastName = lastName;
    }
    sayName() {
      super.breath();
      alert('${this.firstName} ${this.lastName}');
    }
  }

  const newZero = new Zero('human', 'Zero', 'Cho');
  Human.isHuman(newZero); //true
~~~

# promise
- JS와 노드의 API들이 콜백 대신 프로미스 기반으로 재구성. 콜백 지옥현상 극복.

- 실행은 바로 하되 결과값은 나중에 받는 객체. 결과값은 실행이 완료된 후 then이나 catch 메서드를 통해 받는다. (new Promise는 바로 실행되지만 결과값은 then을 붙였을 때 받음.)

~~~
const condition = true; // true면 resolve, false면 reject

const promise = new Promise((resolve, reject)) => {
  if (condition) {
    resolve('성공');
  } else {
    reject('실패');
  }
});
// 다른 코드가 들어갈 수 있음.

Promise
  .then((message) => {
    console.log(message); // 성공(resolve)한 경우 실행
  })
  .catch((error) => {
    console.error(error); // 실패(reject)한 경우 실행
  })
  .finally(() => { //끝나고 무조건 실행
    console.log('무조건')
  });
~~~

- 콜백을 쓰는 패턴 중 하나를 프로미스로 바꾸기

[ES 5]
~~~
function findAndSaveUser(Users) {
  Users.findOne({}, (err, user) = > { // 첫번째 콜백
    if (err) {
      return console.error(err);
    }
    user.name = 'zero';
    user.save((err) => { // 두번째 콜백
      if (err) {
        return console.error(err);
      }
      Users.findOne({ gender: 'm'}, (err, user) => {
        //세번째 콜백
        // 생략
        });
      });
  });
}
~~~

[ES 2015 플러스]
위 코드는 콜백 함수가 3번이나 중첩되었고 콜백 함수가 나올 때마다 코드의 깊이가 깊어진다. 각 콜백 함수마다 에러도 따로 처리해줘야되므로 코드를 간결하게 바꿀 필요가 있다.

~~~
function findAndSaveUser(Users) {
  Users.findOne({})
    .then((user) => {
      user.name = 'zero';
      return user.save();
      })
      .then((user)) => {
        return Users.findOne({ gender: 'm'});
      }
      .then((user)) => {
        //생략
      }
      .catch(err => {
        console.error(err);
        });
}
~~~

- 프로미스 여러개를 한번에 실행할 수 있는 방법

~~~
const promise1 = Promise.resolve('성공1');
const promise2 = Promise.resolve('성공2');
Promise.all([promise1, promise2])
  .then((result)) => {
    console.log(result); // ['성공1', '성공2']
  })
  .catch((eroor) => {
    console.error(error);
    });
~~~

# async/await
프로미스가 콜백 지옥을 해결했다지만, then과 catch가 계속 반복되며 여전히 코드가 길다. async/await 문법은 코드를 더 깔끔하게 만들 수 있다.

- 프로미스 앞에 await를 붙여서 프로미스가 resolve 될 때까지 기다린 다음 user 변수를 초기화.

- try / catch 문으로 로직을 감싸 에러 처리


~~~
const findAndSaveUser = async (Users) => {
  try {
    let user = await Users.findOne({});
    user.name = 'zero';
    user = await user.save();
    user = await Users.findOne({gender :'m'});  
    //생략
  } catch (error) {
      console.error(error);
  }
};
~~~


- for문과 async/await를 같이 써서 프로미스를 순차적으로 실행 가능

~~~
const promise1 = Promise.resolve('성공1');
const promise2 = Promise.resolve('성공2');
Promise.all([promise1, promise2])
  (async () => {
    for await (promise of [promise1, promise2]) {
      console.log(promise);
    }
  })();
~~~
