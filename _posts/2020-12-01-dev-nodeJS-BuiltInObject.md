---  
layout: post
title: nodeJS - 내장 객체(global, console)
categories: dev
tags: nodeJS
comments: true
---

내장 객체 및 내장 모듈 : 따로 설치하지 안항도 바로 사용가능한 객체와 모듈.

- global : 전역 객체. 생략 가능.

globalA.js
```javascript
// 함수 모듈. global.message를 반환하는 함수
module.exports = () => global.message;
```

globalB.js
```javascript
// 함수 모듈 globalA를 변수 A에 저장
const A = require('./globalA');

// globalB에서 넣은 global.message 값을 globalA에서도 접근 가능
global.message = '안녕하세요';
// A(=globalA) 함수 호출
console.log(A());
// 안녕하세요
```

### 주의 global 객체 남용 금지
global 객체(Object)의 속성(property=message)에 값(value='안녕하세요')을 대입하여 파일 간에 데이터를 공유 가능하지만 남용하면 안된다. 프로그램 규모가 커지는 경우 어떤 파일에서 global 객체의 값을 대입했는지 찾기 힘들어져 유지 보수에 어려움을 겪게된다. 만약 **다른 파일의 값을 사용하고 싶다면, 모듈 형식으로 만들어서 명시적으로 값을 불러와 사용하는 것이 좋다.**

- console : 디버깅을 위해 사용. 변수에 값이 제대로 들어갔는지, 에러 발생시 에러 내용 확인 시, 코드 실행 시간을 알아보려고 할 때 사용된다.

console.js
```javascript
const string = 'abc';
const number = 1;
const boolean = true;
const obj {
    outside: {
        inside: {
            key: 'value',
        }
    }
}

console.time('전체 시간')

// console.log(내용) : 괄호 안에 있는 내용을 console에 표시.
console.log('괄호 안에 있는 내용을 표시');
// 괄호 안에 있는 내용을 표시

// console.log(내용) : 쉼표로 구분해 여러 값을 찍을 수 있다.
console.log(string, number, boolean);
// abc 1 true

// console.error(에러 내용):에러 내용을 console에 표시
console.error('에러 메시지는 console.error에 담아주세요');
// 에러 메시지는 console.error에 담아주세요

// console.table(배열) : 배열의 요소로 객체 리터럴을 넣으면, 객체의 속성들이 테이블 형식으로 표현
console.table([{ name: '제로', birth: 1994 }, { name: 'hero', birth: 1998}])
// ┌─────────┬────────┬───────┐
// │ (index) │  name  │ birth │
// ├─────────┼────────┼───────┤
// │    0    │ '제로'  │ 1994  │
// │    1    │ 'hero' │ 1998  │
// └─────────┴────────┴───────┘

// console.dir(객체, 옵션): 객체를 콘솔에 표시 할 때 사용. 옵션의 colors를 true로 하면 콘솔에 색이 추가. depth는 객체 안의 객체를 몇 단계 까지 보여줄지를 결정하며 기본값은 2다.
console.dir(obj, {colors:false, depth: 2});
// { outside: { inside: { key: 'value' } } }
console.dir(obj, {colors:true, depth: 1});
// { outside: { inside: [Object] } }

// console.time(레이블) : console.timeEnd(레이블)과 대응되어 같은 레이블을 가진 time과 timeEnd 사이의 시간을 측정한다.
console.time('시간 측정');
for(let i = 0; i < 100000; i++){}
console.timeEnd('시간 측정');
// 시간 측정: 2.732ms

// console.trace(레이블): 에러가 어디서 발생했는지 추적. 일반적으로 에러 발생시 에러 위치를 알려주므로 자주 사용하지 않지만, 위치가 나오지 않는 경우 사용하면 좋다.
function b() {
    console.trace('에러 위치 추적')
}

function a() {
    b();
}
a();
// Trace: 에러 위치 추적
//     at b (/Users/CREAMer/Desktop/Node JS/console/console.js:33:13)
//     at a (/Users/CREAMer/Desktop/Node JS/console/console.js:37:5)
//     at Object.<anonymous> (/Users/CREAMer/Desktop/Node JS/console/console.js:39:1)
//     at Module._compile (internal/modules/cjs/loader.js:1015:30)
//     at Object.Module._extensions..js (internal/modules/cjs/loader.js:1035:10)
//     at Module.load (internal/modules/cjs/loader.js:879:32)
//     at Function.Module._load (internal/modules/cjs/loader.js:724:14)
//     at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:60:12)
//     at internal/main/run_main_module.js:17:47

console.timeEnd('전체 시간')
// 전체 시간: 16.730ms
```

