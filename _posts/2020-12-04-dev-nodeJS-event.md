---  
layout: post
title: nodeJS - 이벤트
categories: dev
tags: node
comments: true
---

on('data', 콜백) : 'data'라는 이벤트가 발생할 떄 콜백 함수 호출.
on('end', 콜백) : 'end'라는 이벤트가 발생할 떄 콜백 함수 호출.

- 이벤트 만들고, 호출하고, 삭제하기 

event.js

```js
// 모듈 events 불러오기
const EventEmitter = require('events');
const { off } = require('process');

// myEvent 객체 만들기
const myEvent = new EventEmitter();

// .addListener('이벤트 이름', 콜백) : on 기능과 같다.
myEvent.addListener('event1', () => {
    console.log('이벤트 1');
});

// on('이벤트 이름', 콜백) : 이벤트 명과 이벤트 발생 시의 콜백을 연결합니다. 이렇게 연결하는 동작을 event listening이라고 부른다. 
myEvent.on('event2', () => {
    console.log('이벤트 2');
});

// 하나에 이벤트 명에 여러개의 이벤트를 추가할 수 있다.
myEvent.on('event2', () => {
    console.log('이벤트 2 추가');
});

// once('이벤트 이름', 콜백) : 한번만 실행되는 이벤트. 이벤트 이름을 두번 호출(emit)해도 딱 한번만 실행된다.
myEvent.once('event3', () => {
    console.log('이벤트 3');
});

// emit('이벤트 이름') : 이벤트를 호출하는 메서드. 이벤트 이름을 인수로 넣으면 미리 등록해뒀던 콜백 함수가 '한번만 실행'
myEvent.emit('event1');
// 호출 됨 : 이벤트 1
myEvent.emit('event2');
// 호출 됨 : 이벤트 2
myEvent.emit('event3');
// 호출 됨 : 이벤트 3
myEvent.emit('event3');
// 실행 안됨

myEvent.addListener('event4', () => {
    console.log('이벤트 4')
});

// removeAllListeners('이벤트 이름') : 이벤트에 연결된 모든 이벤트 리스너 제거. 
myEvent.removeAllListeners('event4');

myEvent.emit('event4');
// 호출 전에 삭제 했으므로 실행 안됨

const listener = () => {
  console.log('이벤트 5'); 
};

myEvent.on('event5', listener);
// removeListener('이벤트 이름', 리스너) : 이벤트에 연결된 리스너를 하나씩 제거. 리스너를 꼭 넣어야한다.
// = off('이벤트 이름', 콜백)과 기능이 같다.
myEvent.removeListener('event5', listener);
myEvent.emit('event5');
// 실행 안됨

// listenerCount: 이벤트에 현재 리스너가 몇개 연결되었는지 확인
console.log(myEvent.listenerCount('event2'));
// 1

```