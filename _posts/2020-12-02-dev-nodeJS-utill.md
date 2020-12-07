---  
layout: post
title: nodeJS - built-in module (utill)
categories: dev
tags: node
comments: true
---

utill : 각종 편의 기능을 모아둔 module

utill.js

```js
const util = require('util');
const crypto = require('crypto');

// Syntax
// util.deprecate : 함수가 deprecate 처리 되었음을 알린다.
const 변수 = util.deprecate((x, y)) => {
    console.log(x + y);
// 첫번째 인수로 넣은 함수를 사용 했을 때 경고 메시지 출력. 두번째 인수로 경고 메시지를 넣으면 된다.
}, '에러메시지' );

const dontUseMe = util.deprecate((x, y) => {
    console.log(x + y);
}, 'dontUSeMe 함수는 deprecated 되었으니 더 이상 사용하지 마세요!' );
// deprecated 함수 : 중요도가 떨어져 더 이상 사용되지 않고 앞으로는 사라지게 될 함수. 
// 이전 사용자를 위해 기능을 제공하지만 곧 없앨 예정이므로 더 이상 사용하지 말라는 의미


// util.promisify : 콜백 패턴을 promise 패턴으로 바꾼다. 바꿀 함수를 인수로 제공하면된다.

const radomBytesPromise = util.promisify(crypto.randomBytes);
radomBytesPromise(64)
    .then((buf) => {
        console.log(buf.toString('base64'));
    })

    .catch((error) => {
        console.error(error)
    });

// YnPPP2TafH+PRJLdjUFs4aIOA9cn8nfbw7cabOo0dg1du0dC4v3CXTDOzQFUQ0yfhAHwU6mB5OwLQ/1LjUnCcQ==
```