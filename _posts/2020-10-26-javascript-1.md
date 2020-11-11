---  
layout: post
title: nodeJS - 핵심 개념
subtitle: 서버, 노드, 런타임, 이벤트, 루프, 논블로킹
categories: dev
tags: javascript
comments: true  
--- 

- **Server** : 네트워크를 통해 클라이어늩에 정보나 서비스를 제공하는 컴퓨터 또는 프로그램. 데이터가 저장되어있는 곳.

- **Client** : 요청을 보내는 주체. (브라우저, 데스크톱 프로그램, 모바일 앱, 서버도 요청을 보내면 클라이언트가 될 수 있다.)

- **Node** : 자바스크립트 런타임. 자바스크립트 프로그램 실행기.

- **RunTime** : 특정 언어로 만든 프로그램들을 실행할 수 있는 환경.

- **Event-driven(이벤트 기반)** : 이벤트가 발생할 때 미리 저장해둔 작업을 수행하는 방식. (이벤트 클릭, 네트워크 요청 등).

- **Event Listener에 콜백(callback) 함수 등록** : 이벤트 기반 시스템에서 특정 이벤트가 발생할 때 무엇을 할지 미리 등록.

- **Event Loop** : 여러 이벤트가 동시에 발생했을 때 어떤 순서로 콜백 함수를 호출할지를 이벤트 루프가 판단.  노드는 자바스크립트 코드의 맨 위부터 한 줄 씩 실행. 노드가 종료 될 때까지 이벤트 처리 작업 반복.

~~~
function first() {
  second();
  console.log('첫 번째');
}
function second() {
  third();
  console.log('두 번째');
}
function third() {
  console.log('세 번째');
}
first();

호출 스택 : anonymous(전역 컨텍스트) -> first() -> second()-> third().
콘솔 : 세 번째 두 번째 첫 번째
~~~

- **Background** : setTimeout 같은 타이머나 이벤트 리스너들이 대기 하는 곳.

- **Task queue(Callback queue)** : 이벤트 발생 후, 백그라운드에서는 태스크 큐로 콜백 함 수를 보냄.

- **논 블로킹 I(input)/O(output)** : 파일 시스템 접근, 네트워크를 통한 요청 작업 등에서 이전 작업이 완료 될 때까지 대기하지 않고 다음 작업을 수행. 작업 성능이 빨라짐. 논 블로킹 방식으로 코딩하는 습관 필요.

~~~
블로킹 방식의 코드
function longRunningTask(){
  console.log('작업 끝')
}

console.log('시작');
longRunningTask();
console.log('다음 작업');

콘솔 결과 : 시작 작업 끝 다음 작업

논블로킹 방식의 코드
function longRunningTask(){
  console.log('작업 끝')
}

console.log('시작');
<!-- setTimeout은 논 블로킹 코드로 만들기 위해 자주 사용되는 방법 중 하나 -->
setTimeout(longRunningTask, 0);
console.log('다음 작업');

콘솔 결과 : 시작 다음 작업 작업 끝
~~~

- **프로세스** : 운영체제에서 할당하는 작업 단위. 프로세스는 스레드를 여러개 생성해 여러 작업을 동시에 처리 가능.

- **스레드** : 프로세스 내에서 실행되는 흐름의 단위. 부모 프로세스의 자원을 공유.

- **싱글 스레드** : 스레드가 하나.

### 서버로서의 Node 장/단점

<table>
    <tr>
      <th> 장점 </th>
      <th> 단점 </th>
    </tr>
    <tr>
      <td> 멀티 스레드 방식에 비해 적은 컴퓨터 자원 사용 </td>
      <td> 기본적으로 싱글 스레드라서 CPU 코어를 하나만 사용 </td>
    </tr>
    <tr>
      <td> I/O 작업이 많은 서버로 적합 </td>
      <td> CPU 작업이 많은 서버로는 부적합 </td>
    </tr>
    <tr>
      <td> 멀티스레드 방식보다 쉬움 </td>
      <td> 하나 뿐ㄴ인 스레드가 멈추지 않도록 관리가 필요함 </td>
    </tr>
    <tr>
      <td> 웹 서버 내장 </td>
      <td> 서버 규모가 커졌을 때 서버 관리 어려움 </td>
    </tr>
    <tr>
      <td> 자바스크립트 사용 </td>
      <td> 어중간한 성능 </td>
    </tr>
    <tr>
      <td> JSON 형식과 쉽게 호> </td>
      <td> 어중간한 성능 </td>
    </tr>
</table>

### 서버 외의 Node
노드는 웹, 모바일, 데스크톱 애플리케이션 개발에도 사용.

노드 기반으로 돌아가는 대표적인 웹 프레임워크는 앵큘러, 리액트, 뷰 등이 있음.

- 앵귤러 : 구글 진영에서 프론트엔드 앱을 만들 때 주로 사용
- 리액트 : 페이스북 진영.
- 리액트 네이티브 : 모바일 개발 도구 (페이스북, 인스타, 테슬라 등)
- 일렉트론 : 데스크톱 개발 도구(아톰, 슬랙, 디스코드)
