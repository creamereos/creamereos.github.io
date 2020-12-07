---  
layout: post
title: nodeJS - 동기 메서드와 비동기 메서드, 블로킹과 논 블로킹
categories: dev
tags: node
comments: true
---

노드는 대부분의 method를 비동기 방식으로 처리한다. 
하지만 몇몇 method는 동기 방식으로 사용 가능하다.(특히 fs 모듈)

- 같은 파일 여러번 읽기

readme2.txt

```txt
저를 여러번 읽어보세요,
```

async.js (비동기 처리 방식)

```js
// 0.모듈 fs 불러오기
const fs = require('fs');

// 1.시작 출력
console.log('시작');

// 2.파일 읽기만 (비동기 method)
// 4.읽기 완료 후 백그라운드가 다시 메인 스레드에 알림. 콜백 함수 실행
fs.readFile('./readme2.txt', (err, data) => {
    if (err) {
        throw err;
    }
    console.log('1번', data.toString());
});

// 2.파일 읽기만 (비동기 method)
// 4.읽기 완료 후 백그라운드가 다시 메인 스레드에 알림. 콜백 함수 실행
fs.readFile('./readme2.txt', (err, data) => {
    if (err) {
        throw err;
    }
    console.log('2번', data.toString());
});

// 2.파일 읽기만 (비동기 method)
// 4.읽기 완료 후 백그라운드가 다시 메인 스레드에 알림. 콜백 함수 실행
fs.readFile('./readme2.txt', (err, data) => {
    if (err) {
        throw err;
    }
    console.log('3번', data.toString());
});

// 3. 끝 출력
console.log('끝')

// [1번째 실행]
// 시작
// 끝
// 3번 저를 여러 번 읽어보세요.
// 2번 저를 여러 번 읽어보세요.
// 1번 저를 여러 번 읽어보세요.

// [2번째 실행]
// 시작
// 끝
// 1번 저를 여러 번 읽어보세요.
// 2번 저를 여러 번 읽어보세요.
// 3번 저를 여러 번 읽어보세요.

// [3번째 실행]
// 시작
// 끝
// 3번 저를 여러 번 읽어보세요.
// 1번 저를 여러 번 읽어보세요.
// 2번 저를 여러 번 읽어보세요.
```

노드에서 같은 코드를 실행했는데 3번 다 결과의 순서가 다르다. 반복 실행할 때마다 결과가 달라진다. **비동기 메서드**들은 백그라운드에 해당 **파일을 읽으라고만 요청**하고 다음 작업으로 넘어간다. 따라서 파일 읽기 요청만 3번을 보내고 console.log('끝')을 출력한다. 나중에 읽기가 완료되면 백그라운드가 다시 메인 스레드에 알린다. 메인 스레드가 그제서야 등록된 콜백 함수를 실행한다.

위와 같은 방식의 장점은 수백개의 I/O 요청이 들어와도 메인 스레드는 백그라운드에 요청처리를 위임한다. 그 후로도 얼마든지 요청을 더 받을 수 있다. 나중에 백그라운드가 각각의 요청 처리가 완료되었다고 알리면 그 때 콜백 함수를 처리하면 된다.

위 코드를 순서대로 처리하고 싶다면 2가지 방법을 사용 할 수 있다.

- 1.동기 메서드

sync.js (동기 처리 방식)

```js
const fs = require('fs');

console.log('시작')

let data = fs.readFileSync('./readme2.txt');
console.log('1번', data.toString());

data = fs.readFileSync('./readme2.txt');
console.log('2번', data.toString());

data = fs.readFileSync('./readme2.txt');
console.log('3번', data.toString());

console.log('끝')

// 시작
// 1번 저를 여러 번 읽어보세요.
// 2번 저를 여러 번 읽어보세요.
// 3번 저를 여러 번 읽어보세요.
// 끝
```

위 코드는 훨씬 더 이해하기 쉽지만 치명적인 단점이 있다. 만약 **readFileSync** 메서드를 사용하면 이전 작업이 완료되어야 다음 작업을 진행할 수 있으므로 요청이 수백 개 이상 들어올때 성능에 문제가 생긴다. 즉, 백그라운드가 작업하는 동안 메인 스레드는 아무것도 하지 못하고 대기하고 있어야 하는 것이다. 이는 메인 스레드가 일을 하지 않고 노는 시간이 생기므로 비효율적이다. 백그라운드는 fs 작업을 동시에 처리할 수 도 있는데, sync 메소드를 사용하면 백그라운드 조차 동시에 처리할 수 없게 된다.

비동기 fs 메서드를 사용해야 백그라운드가 동시에 작업 할 수도 있고, 메인 스레드는 다음 작업을 처리할 수 있다.

동기 메서드의 경우 이름 뒤에 Sync가 붙어 있다. 동기 메서드를 사용해야하는 경우는 극히 드물다. (프로그램을 처음 실행할 때 초기화 용도)

- 2.비동기 메서드

readFile의 콜백 함수에 다음 readFile을 넣으면 된다.

```js
const fs = require('fs');

console.log('시작');
fs.readFile('./readme2.txt', (err, data) => {
    if(err) {
        throw err;
    }
    console.log('1번', data.toString());
    fs.readFile('./readme2.txt', (err, data) => {
        if(err) {
            throw err;
        }
        console.log('2번', data.toString());
        fs.readFile('./readme2.txt', (err, data) => {
            if(err) {
                throw err;
            }
            console.log('3번', data.toString());
            console.log('끝');
        });
    });
});

// 시작
// 1번 저를 여러 번 읽어보세요.
// 2번 저를 여러 번 읽어보세요.
// 3번 저를 여러 번 읽어보세요.
// 끝
```

위 코드는 이른바 콜백 지옥이 펼쳐지지만 순서가 어긋나는 일은 없다. 콜백 지옥은 Promise나 async/await으로 어늦어도 해결 가능하다.

asyncOrderPromise.js

```js
const fs = require('fs').promises;

console.log('시작');

fs.readFile('./readme2.txt')
    .then((data) => {
        console.log('1번', data.toString());
        return fs.readFile('./readme2.txt');
    })
    .then((data) => {
        console.log('2번', data.toString());
        return fs.readFile('./readme2.txt');
    })
    .then((data) => {
        console.log('3번', data.toString());
        console.log('끝')
    })
    .catch((err) => {
        console.error(err);
    });

    // 시작
    // 1번 저를 여러 번 읽어보세요.
    // 2번 저를 여러 번 읽어보세요.
    // 3번 저를 여러 번 읽어보세요.
    // 끝
```

**[동기와 비동기, 블로킹과 논 블로킹]**

- 동기와 비동기 : **백그라운드 작접 완료 확인** 여부

- 블로킹과 논 블로킹 : **함수가 바로 return**되는지 여부

노드에서는 동기-블로킹 방식과 비동기-논 블로킹 방식이 대부분이다. (동기-논 블로킹, 비동기-블로킹은 거의 없다고 보면 된다.)

![](/assets/img/post/2020-12-04-15-18-56.png)

- **동기-블로킹 방식** : 백그라운드 작업 완료 여부를 계속 확인하며, 호출한 함수가 바로 return되지 않고 백그라운드 작업이 끝나야 return.

- **비동기-논블로킹 방식** : 호출한 함수가 바로 return 되어 다음작업으로 넘어가며, 백그라운드 작업 완료 여부는 신경쓰지 않고 나중에 백그라운드가 알림을 줄때 비로소 처리한다.