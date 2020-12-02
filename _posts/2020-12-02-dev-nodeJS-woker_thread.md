---  
layout: post
title: nodeJS - worker_threads
categories: dev
tags: nodeJS
comments: true
---

worker_threads : 노드에서 멀티 스레드 방식으로 작업 가능한 모듈

worker_thread.js

```js
// 각 변수에 모듈 불러오기
const {
    Worker, isMainThread, parentPort
} = require('worker_threads');

// isMainThread를 통해 현재 코드가 메인스레드(=기존에 동작하던 싱글 스레드=부모스레드)에서 실행되는지 밑에서 생성한 워커 스레드에서 생성 되는지 구분
if (isMainThread) {
    // 메인 스레드에서는 new Worker를 통해 현재 파일(__filename)을 워커 스레드(else 부분)에서 실행 시키고 있다.
    const worker = new Worker(__filename);
    // 부모는 worker.on('message')로 메시지를 받는다. (참고 : 메시지를 한번만 받고싶다면 once('message')를 사용하면 된다.)
    worker.on('message', message => console.log('from worker', message));

    // 주의 사항 : 워커에서 on 메서드를 사용 할 때는 직접 워커를 종료해야한다. parentPort.close()를 하면 부모와의 연결이 종료된다.
    // 종료 될 때는 worker.on('exit')이 실행
    worker.on('exit', () => console.log('worker exit'));

    // 부모 스레드에서 New Worker를 생성후, worker.postMessage로 워커에 데이터를 보낼 수 있다.
    worker.postMessage('ping');
}  
    // 워커일 때
    else {
    // 워커는  parentPort.on(message) 이벤트 리스너로 부모로부터 메시지를 받고, .
    parentPort.on('message', (value) => {
        console.log('from parent', value);
        // parentPort.postMessage로 부모에게 메시지를 보낸다
        parentPort.postMessage('pong');
        parentPort.close();
    });
}
```

worker_data.js

```js
const {
    Worker, isMainThread, parentPort, workerData,
} = require('worker_threads');

if (isMainThread){
    const threads = new Set();
    // 1.new Worker 호출 시 
    threads.add(new Worker(__filename, {
        // 2.2번째 인수의 workerData 속성으로 원하는 데이터를 보낼 수 있다.
        workerData: { start: 1},
    }));

    threads.add(new Worker(__filename, {
        workerData: { start: 2},
    }));

    for (let worker of threads) {
        // 6.돌려주는 순간 워커가 종료되어 worker.on('exit')이 실행된다. 
        worker.on('message', message => console.log('from worker', message));
        worker.on('exit', () => {
            threads.delete(worker);
            if (threads.size === 0) {
                // 7.워커 두개가 모두 종료되면 job done이 로깅 된다.
                console.log('job done');
            }
        });
    }
} 
    // 3.워커일 때
    else {
        // 4.workerData로 부모로부터 데이터를 받는다. 현재 2개의 워커가 돌아가고 있으며, 
        const data = workerData;
        // 5.각각 부모로부터 숫자를 받아서 100을 더해 돌려준다.
        parentPort.postMessage(data.start + 100);
}

// from worker 101
// from worker 102
// job done
```

**[워커 스레드 미사용 vs 워커스레드 사용]**
2부터 1000만까지의 숫자 중 소수가 모두 몇개 있는지를 알아내는 코드

no-workerthread.js

```js
const { start } = require("repl");

const min = 2;
// 최대 값 : 천만
const max = 10000000; 
const primes = [];

function generatePrimes(start, range) {
    let isPrime = true;
    const end = start + range;
    for (let i = start; i < end; i++){
        for (let j = min; j < Math.sqrt(end); j++){
            if (i !== j && i % j ===0){
                isPrime = false;
                break;
            }
        }
        if (isPrime) {
            primes.push(i);
        }
        isPrime = true;
    }
}

console.time('prime');
generatePrimes(min, max);
console.timeEnd('prime');
console.log(primes.length);

// prime: 14888.368ms (15초)
// 664579
```

- 멀티 스레드 : 속도가 빨라진다. 하지만 스레드를 생성하고 스레드 사이에서 통신하는데 상당한 비용이 발생하므로 이 점을 고려해 멀티 스레딩을 해야한다. 잘못하면 멀티 스레딩을 할 때 싱글 스레딩 보다 더 느려지는 현상이 발생할 수도 있다.

use-workerthread.js

```js
const { worker } = require('cluster');
const { Worker, isMainThread, parentPort, workerData } = require('worker_threads');

const min = 2;
let primes =[];

function findPrimes(start, range) {
    let isPrime = true;
    let end = start + range;
    for (let i = start; i < end; i++){
        for (let j = min; j < Math.sqrt(end); j++){
            if (i !== j && i % j ===0){
                isPrime = false;
                break;
            }
        }
        if (isPrime) {
            primes.push(i);
        }
        isPrime = true;
    }
}

if (isMainThread) {
    const max = 10000000;
    const threadCount = 8;
    const threads = new Set();
    const range = Math.ceil((max - min) / threadCount);
    let start = min;
    console.time('prime');
    for (let i = 0; i < threadCount -1; i++){
        const wStart = start;
        threads.add(new Worker(__filename, { workerData: { start: wStart, range } }));
        start += range;
    }
    threads.add(new Worker(__filename, { workerData: { start, range: range + ((max - min + 1) % threadCount) } }));

    for (let worker of threads) {
        worker.on('error', (err) => {
            throw err;
        });
        worker.on('exit', () => {
            threads.delete(worker);
            if (threads.size === 0) {
                console.timeEnd('prime');
                console.log(primes.length);
            }
        });
        worker.on('message', (msg) => {
            primes = primes.concat(msg);
        });
    }
} else {
    findPrimes(workerData.start, workerData.range);
    parentPort.postMessage(primes);
}


// prime: 6083.821ms (6초)
// 664579
```
