---  
layout: post
title: nodeJS - 파일 시스템 접근하기 (fs module)
categories: dev
tags: node
comments: true
---

fs module : 파일 시스템에 접근하는 모듈. 즉 파일을 생성/삭제/읽기/쓰기, 폴더 생성/삭제 가능. 

- 파일 읽기

readme.txt

```txt
readame 파일 읽기
```

readFile.js

```js
const fs = require('fs');

// 읽을 파일의 경로 지정.
fs.readFile('./readme.txt', (err, data) => {
    if (err) {
        throw err;
    }
    // Buffer 데이터
    console.log(data);
    // Buffer는 사람이 읽을 수 있는 형식이 아니므로 toString을 사용해 문자열로 변환
    console.log(data.toString());
});

// <Buffer 46 53 20 eb aa a8 eb 93 88 20 ed 85 8c ec 8a a4 ed 8a b8 2e 20 ec 9d bd ea b8 b0>
// FS 모듈 테스트. 읽기
```

- fs는 기본적으로 콜백 형식의 모듈이므로 실무에서 사용하기가 불편. 따라서 **fs 모듈을 프로미스 형식으로 변환**

readFilePromise.js

```js
const fs = require('fs').promises;

fs.readFile('./readme.txt')
    .then((data) => {
        console.log(data);
        console.log(data.toString());
    })

    .catch((err) => {
        console.error(err);
    });

// <Buffer 46 53 20 eb aa a8 eb 93 88 20 ed 85 8c ec 8a a4 ed 8a b8 2e 20 ec 9d bd ea b8 b0>
// FS 모듈 테스트. 읽기
```

- 파일 생성


```js
const fs = require('fs').promises;

// 파일 생성
fs.writeFile('./writeme.txt', '파일 생성 및 텍스트 입력')
    .then(() => {
        // 파일 읽기
        return fs.readFile('./writeme.txt')
    })

    .then((data) => {
        // 콘솔 창에 텍스트 출력
        console.log(data.toString());
        // console 창에 '파일 생성 및 텍스트 입력' 출력.
    })

    .catch((err) => {
        console.error(err);
    });
```