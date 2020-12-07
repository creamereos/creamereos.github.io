---  
layout: post
title: nodeJS - REPL & JS 파일 실행
categories: dev
tags: node
comments: true
---

### REPL 사용하기 (짧은 코드)
- REPL(Read/Eval/Print/Loop) : 입력한 코드를 Read(읽고), Eval(해석하고), Print(결과물을 반환하고), Loop(종료할 때 까지 반복한다.)
자바스크립트는 스크립트 언어이므로 미리 컴파일 하지 않아도 터미널에서 즉석으로 코드 실행 가능. 

- 노드 실행 방법 (터미널)
1. VS CODE에서 컨트롤+`
2. node 입력 (Node가 설치되어있어야한다.)
3. 프롬포트의 모양이 > 로 바뀌었다면 자바스크립트 코드 입력 가능

- 노드 종료 방법
1. 컨트롤+CC or 터미널에 .exit 입력

REPL(터미널)은 1~2줄의 코드를 테스트해보는 용도. 긴코드의 경우 자바스크립트 파일을 별도로 만든후 실행하는 것이 좋다.

### JS 파일 실행하기 (긴 코드)

- JS 파일 생성 

```javascript
function helloWorld() {
    console.log('Hello World');
    helloNode();
}

function helloNode() {
    console.log('Hello Node');
    helloWorld();
}

helloWorld(); 
```

- REPL이 아닌 console에서 JS 파일 실행(현재 폴더 확인하기)
console에서 REPL로 들어가는 명령어가 node


```console
$ node helloWorld
노드를 통해 파일을 실행하는 명령어는 node 자바스크립트 파일명(확장자(.js) 생략 가능)

// Hello World
// Hello Node
```
