---  
layout: post
title: nodeJS - module, exports, require
categories: dev
tags: nodeJS
comments: true
---

exports 객체로도 모듈 생성 가능.

- var.js
```javascript
exports.odd = '홀수';
exports.even = '짝수';
// 위 코드는 아래 코드와 같다.
// module.exports = {
//     odd,
//     even,
// }

console.log(module.exports === exports)
// true
```

- exports와 module.exports의 reference 관계

exports **참조(reference)->**  module.exports **참조(reference)->** 객체{}

**[exports 객체 사용시 주의 사항]**
- module.exports와의 참조(reference) 관계가 깨지지 않도록 주의 : module.exports에는 어떤 값이든 대입 가능. 그러나 exports에는 반드시 객체 처럼 속성명과 속성값을 대입해야한다. exports에 다른 값을 대입하면 객체(Object)의 레퍼런스 관계가 끊겨 더 이상 모듈로 기능하지 않는다.

```javascript
exports.속성명 = '속성값';
```

- exports를 사용할 때는 객체만 사용 가능. module.exports에 함수를 대입한 경우 exports로 바꿀 수 없다.

```javascript
function 함수명() {
    실행할 코드
}

// 다른 모듈(var.js)를 사용하는 파일을 다시 모듈(func.js)로 만들 수 있다.
module.exports = 함수명;
// 'exports =함수명;'로 변경 불가능
```

- exports와 module.exports는 reference 관계에 있으므로 한 모듈에 exports 객체와 module.exports를 동시에 사용하지 않는 것이 좋다.

### Node에서 this 사용 시 주의점

```javascript
console.log(this);
// {}
// this는 객체를 가리킨다.
console.log(this === module.exports);
// true
// this는 module.exports를 가리킨다.
console.log(this === exports)
// true
// this는 exports를 가리킨다.

function whatIsThis(){
    console.log('function', this === exports, this === global);
}
whatIsThis();
// function false true
// 함수 선언문 내부의 this는 global 객체를 가리킨다.
```

### require : 모듈을 불러오는 함수
require는 함수이고, 함수는 객체이므로 객체로서 몇가지 속성을 가지고 있다. 

위 예제 코드에서 알아야 할 점은 **1.require는 가장 위에 오지 않아도 된다.**, **2.module.exports도 최하단에 위치할 필요가 없다.** 둘 다 아무곳에서나 사용 해도 된다.

```javascript
console.log('require는 가장 위에 오지 않아도 된다.');

module.exports = '저를 찾아보세요';

// var.js 모듈 불러오기
require('./var');

console.log('require.cache')
console.log(require.cache);
console.log('require.main')
console.log(require.main);

console.log(require.main === module);
console.log(require.main.filename);
```

- require.cache : require한 파일은 require.cache에 저장되므로 다음 번에 require를 할 떄 새로 불러오지 않고, require.cache에 있는 것이 재사용된다. (만약 새로 require하길 원한다면 require.cache 속성을 제거하면 된다. 다만 프로그램의 동작이 꼬일 수 있으므로 권장하지 않는다.)

```javascript
[Object: null prototype] {
  '/Users/CREAMer/Desktop/Node JS/module/require.js': Module {
    id: '.',
    path: '/Users/CREAMer/Desktop/Node JS/module',
    exports: '저를 찾아보세요',
    parent: null,
    filename: '/Users/CREAMer/Desktop/Node JS/module/require.js',
    loaded: false,
    children: [ [Module] ],
    paths: [
      '/Users/CREAMer/Desktop/Node JS/module/node_modules',
      '/Users/CREAMer/Desktop/Node JS/node_modules',
      '/Users/CREAMer/Desktop/node_modules',
      '/Users/CREAMer/node_modules',
      '/Users/node_modules',
      '/node_modules'
    ]
  },
  '/Users/CREAMer/Desktop/Node JS/module/var.js': Module {
    id: '/Users/CREAMer/Desktop/Node JS/module/var.js',
    path: '/Users/CREAMer/Desktop/Node JS/module',
    exports: { odd: '홀수', even: '짝수' },
    parent: Module {
      id: '.',
      path: '/Users/CREAMer/Desktop/Node JS/module',
      exports: '저를 찾아보세요',
      parent: null,
      filename: '/Users/CREAMer/Desktop/Node JS/module/require.js',
      loaded: false,
      children: [Array],
      paths: [Array]
    },
    filename: '/Users/CREAMer/Desktop/Node JS/module/var.js',
    loaded: true,
    children: [],
    paths: [
      '/Users/CREAMer/Desktop/Node JS/module/node_modules',
      '/Users/CREAMer/Desktop/Node JS/node_modules',
      '/Users/CREAMer/Desktop/node_modules',
      '/Users/CREAMer/node_modules',
      '/Users/node_modules',
      '/node_modules'
    ]
  }
}
```

- require.main : 노드 실행시 첫 모듈을 가리킨다. 

```javascript
// console.log(require.main);
Module {
  id: '.',
  path: '/Users/CREAMer/Desktop/Node JS/module',
  exports: '저를 찾아보세요',
  parent: null,
  filename: '/Users/CREAMer/Desktop/Node JS/module/require.js',
//  노드 실행시 첫 모듈을 가리킨다. 
  loaded: false,
  children: [
    Module {
      id: '/Users/CREAMer/Desktop/Node JS/module/var.js',
      path: '/Users/CREAMer/Desktop/Node JS/module',
      exports: [Object],
      parent: [Circular],
      filename: '/Users/CREAMer/Desktop/Node JS/module/var.js',
      loaded: true,
      children: [],
      paths: [Array]
    }
  ],
  paths: [
    '/Users/CREAMer/Desktop/Node JS/module/node_modules',
    '/Users/CREAMer/Desktop/Node JS/node_modules',
    '/Users/CREAMer/Desktop/node_modules',
    '/Users/CREAMer/node_modules',
    '/Users/node_modules',
    '/node_modules'
  ]
}
```

- 현재 파일이 첫 모듈인지 확인 하는 방법
```javascript
현재 파일 === module
console.log(require.main === module)
// true

```

- 첫 모듈의 이름 확인 방법

```javascript
console.log(require.main.filename);
// /Users/CREAMer/Desktop/Node JS/module/require.js
```

- 모듈 사용 시 주의 사항 : 두 모듈이 서로를 require하는 경우

dep1.js
```javascript
// 모듈 불러오기
const dep2 =require('./dep2');
console.log('require dep2', dep2);

// 모듈화
module.exports = () => {
    console.log('dep2', dep2);
};
```

dep2.js
```javascript
// 모듈 불러오기
const dep1 =require('./dep1');
console.log('require dep1', dep1);

// 모듈화
module.exports = () => {
    console.log('dep1', dep1);
};
```

dep-run.js (dep 모듈을 실행하는 js 파일)
```javascript
// 모듈 불러오기
const dep1 = require('./dep1');
const dep2 = require('./dep2');

// 모듈 실행하기
dep1();
dep2();
```

- 결과
```javascript
require dep1 {}
// dep1.의 module.exports가 함수가 아닌 빈 객체로 표시. 이러한 현상을 '순환 참조'라고한다. 순환 참조가 있을 경우 순환 참조가 되는 대상을 빈 객체로 만든다. 이 때 에러가 발생하지 않고 조용히 빈 객체로 변경되므로 예기치 못한 동작이 발생할 수 있다. 따라서 순환 참조가 발생하지 않도록 구조를 잘잡는 것이 중요하다.
require dep2 [Function]
dep2 [Function]
dep1 {}
```