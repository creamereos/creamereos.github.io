---  
layout: post
title: nodeJS - 예외 처리하기(try/catch) / 자주 발생하는 에러
categories: dev
tags: nodeJS
comments: true
---

노드에서는 예외 처리가 정말 중요하다. 예외란 보통 처리하지 못한 에러를 가리킨다. 예외들은 실행중인 노드 프로세스를 멈추게 만든다.

멀티 스레드에서는 스레드 하나가 멈추면 그 일을 다른 스레드가 대신하지만, 노드의 메인 스레드는 하나 뿐이므로 메인스레드가 에러로 인해 멈추면 프로세스 자체가 멈춘다. 즉, 전체 서버가 멈춘다. 

따라서 에러 처리 방법은 중요하고, 에러 로그가 기록되더라도 작업은 계속 진행 될 수 있게 해야한다.

- try/catch : 에러가 발생할 것 같은 부분을 미리 try/catch로 감싸라.

error.js

```js
// 프로세스가 멈추는지 체크하기 위해 setInterval 사용.
// 프로세스가 에러로 인해 멈추면 setInterval도 멈춘다.
setInterval(() => {
    console.log('start');
    try {
        // throw new Error()를 써서 에러를 강제로 발생
        // throw를 하는 경우 반드시 try/catch 문으로 throw한 에러를 잡아야한다.
        throw new Error('server error!')
    } catch (err) {
        console.error(err);
    }
}, 1000);

// 컨트롤 + C : 터미널 콘솔 실행 종료
```

- 노드 자체에서 잡아주는 에러 : 노드의 내장 모듈(unlink 등)의 에러는 실행중인 프로세스를 멈추지않는다. 에러 로그를 기록해두고 추후 원인을 수정하면된다.

```js
const fs = require('fs');

setInterval(() => {
    // 존재하지 않는 파일(abcdefg.js)을 지우므로(unlink) 에러가 발생하는 코드다.
    fs.unlink('./abcdefg.js', (err) => {
        if (err) {
            console.error(err);
        }
    });
}, 1000);
// 위 코드가 계속 반복
// 컨트롤 + C : 터미널 콘솔 실행 종료
```

- 프로미스의 에러 : catch하지 않아도 알아서 처리됨. 다만 프로미스의 에러를 알아서 처리하는 동작은 노드 버전이 올라감에 따라 빠뀔 수 있으니 **항상 catch를 붙이는 것을 권장한다.**

```js
const fs = require('fs');

setInterval(() => {
    fs.unlink('./abcedfg.js')
}, 1000);
```

- 예측 불가능한 에러 처리 방법


```js
// process 객체에 .on을 통해 'uncaughtException' 이벤트 리스너를 달았다. 
// 처리하지 못한 에러가 발생 했을 때 이벤트 리스너가 실행되고 프로세스가 유지된다. 
// 이 부분이 없으면 아래 예제에서 setTimeou이 실행되지 않는다. 
// 실행 후 1초만에 setInterval에서 에러가 발생하여 프로세스가 멈추기 때문이다. 
// 하지만 'uncaughtException' 이벤트 리스너가 연결되어 있으므로 프로세스는 멈추지 않는다.
process.on('uncaughtException', (err) => {
    console.error('예기치 못한 에러', err);
});

setInterval(() => {
    throw new Error('서버를 고장내주마!');
}, 1000);

setTimeout(() => {
    console.log('실행됩니다.');
}, 2000);
```

위 코드는 try/catch로 처리하지 못한 에러가 발생했지만 코드가 제대로 실행되었다. 어떻게 보면 uncaughtException 이벤트로 모든 에러를 처리 할 수 있는 것처럼 보이지만, 노드 공식 문서에서는 uncaughtException 이벤트를 최후의 수단으로 사용할 것을 명시하고 있다. 

노드는 uncaughtException 이벤트 발생 후 다음 동작이 제대로 작동하는지를 보증하지 않는다. 즉, 복구 작업 코드를 넣어 두었더라도 그것이 동작하는지 확인 할 수 없다.

따라서 ***uncaughtException은 단순히 에러 내용을 기록하는 정도로 사용**하고, **에러를 기록한 후 process.exit()로 프로세스를 종료**하는 것이 좋다. 에러가 발생하는 코드를 수정하지 않는 이상 프로세스가 실행되는 동안 에러는 계속 발생할것이다.

**서버 운영은 에러와의 싸움이다. 따라서 에러 발생시 철저히 기록(로깅)하는 습관을 들이고, 주기적으로 로그를 확인하면서 보완해야한다.**

**[자주 발생하는 에러들]**

- **node: commnad not foun** : 노드를 설치했지만 이 에러가 발생하는 경우는 **환경 변수가 제대로 설정 되지 않은 것**. 환경 변수에는 노드가 설치된 경로가 포함되어야한다. node외에 다른 명령어도 마찬가지이다. 해당 명령어를 수행 할 수 있는 파일이 환경 변수에 들어있어야 명령어를 콘솔에서 사용 할 수 있다.

- **ReferenceError : module is not defined** : module을 require 했는지 확인해야한다.

- **Error: Cannot find module 모듈 이름** : 해당 module을 require 했지만 설치되지 않았다. npm i 커맨드로 설치해야한다.

- **Error: Can't set headers after they are sent** : 요청에 대한 응답을 보낼 때 응답을 두번 이상 보냈다. 요청에 대한 응답은 한번만 보내야한다. 응답을 보내는 메서드를 두 번 이상 사용하지 않았는지 체크한다.

- **FATAL ERROR: CALL_AND_RETRY_LAST Allocation failed - Javascript heap out of memory** : 코드를 실행할 때 메모리가 부족하여 스크립트가 정상 작동하지 않는 경우. 코드가 잘못되었을 확률이 높으므로 코드를 점검해라. 만약 코드가 정상이지만 노드가 활용 할 수 있는 메모리가 부족한 경우라면 노드의 메모리를 늘릴 수 있다. 노드를 실행할 때 **$ node --max-old-space-size=4096 파일명**을 사용하면된다. (4096은 4GB를 의미한다. 원하는 용량을 적으면된다.)

- **UnhandledPromiseRejectionWarning: Unhandled promise rejection** : 프로미스 사용 시 catch 메서드를 붙이지 않으면 발생한다. 항상 catch를 붙여 에러가 나는 상황을 대비해라.

- **EADDRINUSE 포트 번호** : 해당 포트 번호에 이미 다른 프로세스(노드 또는 다른 프로그램)가 연결되어있다. 그 **프로세스를 종료**하거나 다른 포트 번호를 사용해야한다.

**[맥에서 프로세스 종료하기]**

```
<!-- 터미널에 입력 -->
$ lsof -i tcp:포트
<!-- 위 코드를 실행하면 ID가 나오는데 아이디를 다음 코드에 넣고 실행 -->
$ kill -9 프로세스 아이디
<!-- 프로세스 종료 -->
```

- **EACCES 또는 EPERM** : 노드가 작업을 수행하는데 **권한**이 충분하지 않다. 파일/폴더 수정,삭제,생성의 권한을 확인해봐라. 맥에서는 명령어 앞에 **sudo**를 붙이는 것도 방법.

- **EHSONPARSE** : pakage.json 등 JSON 파일에 문법 오류가 있을 때 발생. 자바스크립트 객체와는 형식이 조금 다르니 쉼표 같은게 빠지거나 추가되지는 않았는지 확인해본다.

- **ECONNREFUSED** : 요청을 보냈으나 연결이 성립되지 않을 때 발생. 요청을 받는 서버의 주소가 올바른지, 꺼져있지는 않은지 확인해봐야한다.

- **ETARGET** : pakage.json에 기록한 패키지 버전이 존재하지 않을 때 발생. 해당 버전이 존재하는지 확인해본다.

- **ETIMEOUT** : 요청을 보냈으나 응답이 일정 시간 내에 오지 않을 때 발생. 요청을 받는 서버의 상태를 점검해봐야한다.

- **ENOENT: no such file or directory** : 지정한 폴더나 파일이 존재하지 않는 경우. (맥에서는 대소문자도 구별하니 확인해봐야한다.)

https://nodejs.org