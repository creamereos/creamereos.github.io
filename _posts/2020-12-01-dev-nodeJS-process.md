---  
layout: post
title: nodeJS - process
categories: dev
tags: node
comments: true
---

process 객체는 현재 실행되고 있는 노드 프로세스에 대한 정보를 담고 있습니다. 

- process.env : 시스템의 환경 변수로 노드에 직접 영향을 미치기도 한다. 

```js
> process.env

// 환경 변수 이름 = 값
NODE_OPTIONS = --max-old-space-size=8192
// 노드를 실행할 때의 옵션들을 입력받는 환경 변수
UV_THREADPOOL_SIZE = 8
// 사용하는 스레드풀의 스레드 개수를 조절
```

- process.env는 임의로 환경 변수 저장 가능. 서비스의 중요한 키를 저장하는 곳으로도 사용. 서버나 데이터베이스의 비밀번호와 각종 API키를 코드에 직접 입력하는 것은 위험하다. (서비스가 해킹 당해 코드가 유출되었을 때 비밀번호가 코드에 남아있어 추가 피해가 발생할수 있기 때문에) 따라서 **중요한 데이터(비밀번호 등)은 process.env 속성으로 대체한다.

```js
const secretID = process.env.SECRET_ID;
const secretCode = process.env.SECRET_CODE;

// 이제 process.env에 직접 SECRET_ID와 SECRET_CODE를 넣으면 된다. 
```

- process.nextTick(콜백) : 이벤트루프가 다른 콜백함수들보다 nextTick의 콜백 함수를 우선 처리한다.

```js
setImmediate(()=>{
    console.log('immediate');
})

nextThick(()=>{
    console.log('nextTick');
})

setTimeout(()=>{
    console.log('timeout');
} 0);

Promise.resolve().then(() => console.log('pormise'));
```

- 실행 결과 : 마이크로 태스크(process.nextTick과 resolve된 Promise 우선 실행)

```js
// process.nextTick은 setTimeout과 setImmediate 보다 우선 실행 
nextTick
// resolve된 Promise도 다른 콜백 함수들보다 우선시.
pormise
timeout
immediate
```

- 마이크로 태스크 주의 사항
**비동기 처리시 setImmediate** 보다 process.nextTick을 선호하는 개발자도 있지만 마이크로 태스크(setImmediate, process.nextThick)를 재귀호출 하게되면 이벤트 루프는 다른 콜백 함수보다 마이크로 태스크를 우선 처리하므로 콜백 함수들이 실행되지 않을 수 있다.

- process.exit : 실행중인 노드 프로세스 종료. 
서버 환경에서 process.exit를 사용하면 서버가 멈추므로 특수한 경우를 제외하고는 서버에서 잘 사용하지 않는다.
하지만 서버외 독립적인 프로그램에서는 수동으로 노드를 멈추기 위해 사용.

```js
let i = 1;
setInterval(() => {
    if(i===5){
        console.log('종료!');
        process.exit();
    }
    console.log(i);
    i +=1 ;
}, 1000);

// 1
// 2
// 3
// 4
// 종료!
```

- ETC

```js
// 설치된 노드의 버전
> process.version
'v12.19.0' 

// 프로세서 아키텍처 정보
> process.arch
'x64'

// 운영체제 플랫폼 정보
> process.platform
'darwin'

//현재 프로세스 아이디
> process.pid
163

// 프로세스가 시작된 후 흐른 시간 (단위: 초)
> process.uptime
[Function: uptime]

// 노드의 경로
> process.execPath
'/usr/local/bin/node'

// 현재 프로세스가 실행되는 위치
> process.cwd()
'/Users/Desktop/Node JS'

// 현재 CPU 사용량
> process.cpuUsage
[Function: cpuUsage]
```