---  
layout: post
title: nodeJS - cluster
categories: dev
tags: nodeJS
comments: true
---

cluster : 기본적으로 싱글 프로세스를 동작하는 NODE가 CPU 코어를 모두 사용할 수 있게 해주는 모듈. (멀티 노드 프로세스, 병렬 실행, 요청 분산) 서버에 무리가 덜 가고 성능이 좋아진다. 하지만 메모리를 공유하지 못하는 등의 단점도 있다. 세션을 메모리에 저장하는 경우 문제가 될 수 있다. 이는 레디스 등의 데이터베이스를 도입하여 해결 가능하다.

cluster.js : worker_threads 예제와 모양이 비슷. 다만 스레드가 아니라 프로세스이다. 

```js
const cluster = require('cluster');
const http = require('http');
const numCPUs = require('os').cpus().length;

// 마스터 프로세스 : CPU 개수만큼 워커 프로세스를 만들고, 포트에서 대기
// 요청이 들어오면 워커 프로세스에 요청을 분배
if (cluster.isMaster) {
  console.log(`마스터 프로세스 아이디: ${process.pid}`);
  // CPU 개수만큼 워커를 생산
  for (let i = 0; i < numCPUs; i += 1) {
    cluster.fork();
  }
  // 워커가 종료되었을 때
  cluster.on('exit', (worker, code, signal) => {
    console.log(`${worker.process.pid}번 워커가 종료되었습니다.`);
    console.log('code', code, 'signal', signal);
    cluster.fork();
  });
// 워커 프로세스 : 실질적인 일을 하는 프로세스.
} else {
  // 워커들이 포트에서 대기
  http.createServer((req, res) => {
    res.writeHead(200, { 'Content-Type': 'text/html; charset=utf-8' });
    res.write('<h1>Hello Node!</h1>');
    res.end('<p>Hello Cluster!</p>');
    setTimeout(() => { // 워커 존재를 확인하기 위해 1초마다 강제 종료
      process.exit(1);
    }, 1000);
  }).listen(8086);

  console.log(`${process.pid}번 워커 실행`);
}
```

직접 cluster 모듈로 클러스터링을 구현 할 수도 있지만, **실무에서는 pm2등의 모듈로 cluster 기능을 사용**하곤한다. 
