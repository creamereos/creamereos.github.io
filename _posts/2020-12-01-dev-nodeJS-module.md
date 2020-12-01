---  
layout: post
title: nodeJS - module
categories: dev
tags: nodeJS
comments: true
---

모듈(module) : 특정한 기능을 하는 함수나 변수들의 집합. 

- 모듈은 자체로도 하나의 프로그램이면서 다른 프로그램의 부품으로도 사용 및 재사용 가능(자바스크립트에서 코드를 재사용하기 위해 함수로 만드는 것과 비슷) 보통 파일 하나가 모듈 하나가 되며, 파일별로 코드를 모듈화 할 수 있어 관리하기 편하다.

### 모듈 예시

- var.js : 변수 모듈.

```javascript
const odd = '홀수';
const even = '짝수';

// 모듈 대입 방법 : module.exports = 함수, 객체, 변수 대입 가능
// module.exports에 변수(odd,add)를 담은 객체 대입
// 변수들을 모아둔 module.
module.exports = {
    odd,
    even,
}
```

이제 위 JS 파일은 모듈로서 기능하며, 다른 파일에서 이 파일을 불러오면 module.exports에 대입된 값을 사용 가능

- func.js : 함수 모듈.

```javascript
// 모듈 불러오기
const { odd, even } = require('./var');
// require 함수는 모듈을 불러오는 함수이다.
// require('모듈의 경로')
// 다른 폴더에 있는 다른 파일도 모듈로 사용 가능 (.js / .json) 확장자 생략 가능

// 홀짝 판단하는 함수 선언
function checkOddOrEven(num) {
    // 만약 num이 홀수면, (0이 아닌 정수일때 true이므로)
    if (num % 2) {
        return odd;
    }
    return even;
}

// 다른 모듈(var.js)을 사용하는 파일을 다시 모듈(func.js)로 만들 수 있다.
module.exports = checkOddOrEven;
// 모듈 하나가 여러개의 모듈을 사용가능하다.
```

- index.js : 변수 모듈과 함수 모듈을 사용하는 모듈

```javascript
// 모듈 불러오기
const { odd, even } = require('./var');
// 모듈로부터 값을 불러올때 변수의 이름을 다르게도 저장 가능. 
const checkNumber = require('./func');

function checkStringOddOrEven(str) {
    // 만약 num이 홀수면, (0이 아닌 정수일때 true이므로)
    if(str.length % 2){
        return odd
    }
    return even;
}

console.log(checkNumber(10));
console.log(checkStringOddOrEven('hello'));
```

### ES2015 모듈

- var.js

```javascript
// 모듈 불러오기
import {odd, even} from './var';

function checkOddOrEven(num) {
    if (num % 2){
        return odd;
    }
    return even;
}

// 모듈 이름 설정
export default checkOddOrEven;
```