---  
layout: post
title: nodeJS - built-in module (child_process, etc)
categories: dev
tags: nodeJS
comments: true
---

child_process : 노드에서 다른 프로그램을 실행하고 싶거나 명령어를 수행하고 싶을 때 사용하는 모듈. 즉 다른 언어의 코드를 실행하고 결괏값을 받을 수 있다. 노드 프로세스 외에 새로운 프로세스(child_process)를 띄어서 명령을 수행하고 노드 프로세스에 결과를 알려준다.

- 명령 프롬프트의 명령어인 dir을 노드를 통해 실행

exec.js

```js
const exec = require('child_process').exec;

// 현재 폴더의 파일 목록
var process = exec('ls');

// stdout(표준 출력)
process.stdout.on('data', function(data) {
    console.log(data.toString());
});

// stderr(표준 에러)
process.stderr.on('data', function(data) {
    console.error(data.toString());
});

// exec.js
// spawn.js
// test.py
```

- 파이썬 프로그램 실행

test.py

```py
print('hello python')
```

spawn.js

```js
const spawn = require('child_process').spawn;

// spawn('명령어', '[옵션 배열]')
// 파이썬 코드를 실행하는 명령어 'python', ['test.py']를 spawn을 통해 실행.
var process = spawn('python', ['test.py']);

process.stdout.on('data', function(data) {
    console.log(data.toString());
});

process.stderr.on('data', function(data) {
    console.error(data.toString());
});

// hello python
```

**[exec와 spawn의 차이점]**
exec : 셸을 실행해서 명령어 수행
spawn : 새로운 프로세스를 띄우면서 명령어 실행 (3번째 인수로 { shell:true}를 제공하면 exec 처럼 셸을 실행해서 명령어를 수행)


**[기타 모듈]**

- assert : 값을 비교하여 프로그램이 제대로 동작하는지 테스트

- dns : 도메인 이름에 대한 IP 주소를 얻는데 사용

- net : HTTP보다 로우 레벨인 TCP나 IPC 통신을 할 때 사용

- string_decoder : 버퍼 데이터를 문자열로 바꾸는데 사용

- tls : TLS와 SSL에 관련된 작업을 할 때 사용

- tty : 터미널과 관련된 작업을 할 때 사용

- dgram : UDP와 관련된 작업을 할 때 사용

- v8 :  v8 엔진에 직접 접근 할 때 사용

- vm : 가상 머신에 직접 접근할 때 사용
