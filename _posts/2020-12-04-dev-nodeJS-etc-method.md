---  
layout: post
title: nodeJS - 기타 fs 메소드
categories: dev
tags: node
comments: true
---

fs는 단순히 파일 읽기/쓰기 뿐만 아니라 파일을 생성하고 삭제할 수 있으며 폴더를 생성하고 삭제할 수 있다.

- 폴더 및 파일 생성 하기

fsCreate.js

```js
const fs = require('fs').promises;
const constants = require('fs').constants;
```

- fs.access('경로', 옵션, 콜백) : 폴더나 파일에 접근 할 수 있는지 체크. 두번째 인수로 constants로 상수들을 넣는다. F_OK는 파일 존재 여부, R_OK는 읽기 권한 여부, W_OK는 쓰기 권한 여부를 체크한다. 파일/폴더나 권한이 없다면 에러가 발생하는데 파일/폴더가 없을 때의 에러코드는 ENOENT이다.

```js
fs.access('./folder', constants.F_OK | constants.W_OK | constants.R_OK)
    .then(() => {
        return Promise.reject('이미 폴더 있음');
    })
    .catch((err) => {
        if (err.code === 'ENOENT') {
            console.log('폴더 없음');
            // fs.mkidr('경로', 콜백) : 폴더를 만드는 메서드. 이미 폴더가 있다면 에러가 발생하므로 먼저 access 메서드를 호출해서 확인하는 것이 중요하다.
            return fs.mkdir('./folder');
        }
        return Promise.reject(err);
    })
    .then(() => {
        console.log('폴더 만들기 성공');
        // fs.open(경로, 온셥, 콜백): 파일의 아이디(fd 변수)를 가져오는 메서드. 파일이 없다면 파일을 생성한 뒤 그 아이디를 가져온다. 가져온 아이디를 사용해 읽거나(fs.read) 쓸 수(fs.write) 있다. 
        // 2번째 인수로 어떤 동작을 할 것인지 설정 가능. ('w' 쓰기 / 'r' 읽기 / 'a' 기존 파일에 추가)
        return fs.open('./folder/file.js', 'w');
    })
    .then((fd) => {
        console.log('빈 파일 만들기 성공', fd);
        // fs.rename(기존 경로, 새 경로, 콜백) : 파일의 이름을 바꾸는 메서드. 기존 파일 위치와 새로운 파일 위치를 저으면 된다. 꼭 같은 폴더를 저장할 필요는 없으므로 잘라내기와 같은 기능을 할 수 도 있다.
        fs.rename('./folder/file.js', './folder/newfile.js');
    })
    .then(() => {
        console.log('이름 바꾸기 성공')
    })
    .catch((err) => {
        console.error(err);
    });
```

- 폴더 내용 확인 및 삭제 

fsDelete.js

```js
const fs = require('fs').promises;

// fs.readdir(경로, 콜백) : 폴더 안의 내용물을 확인. 배열 안에 내부 파일과 폴더명이 나온다.
fs.readdir('./folder')
    .then((dir) => {
        console.log('폴더 내용 확인', dir);
        // 폴더 내용 확인 [ 'newfile.js' ]

        // fs.unlink(경로, 콜백): 파일을 삭제한다. 파일이 없다면 에러가 발생하므로 파일이 있는지 꼭 확인해야한다.
        return fs.unlink('./folder/newFile.js');
    })
    .then(() => {
        console.log('파일 삭제 성공')
        // fs.rmdir(경로, 콜백) : 폴더를 삭제한다. 폴더 안에 파일들이 있다면 에러가 발생하므로 먼저 내부 파일을 모두 지우고 호출해야한다.
        return fs.rmdir('./folder');
    })
    .then(() => {
        console.log('폴더 삭제 성공');
    })
    .catch((err) => {
        console.error(err);
    });
```

위 코드를 한번 더 실행($ node fsDelete)하면 (존재하지 않는 파일을 지웠으므로) ENOENT 에러가 발생 

```js
[Error: ENOENT: no such file or directory, scandir './folder'] {
  errno: -2,
  code: 'ENOENT',
  syscall: 'scandir',
  path: './folder'
}
```

- 파일 복사하기2

노드 8.5 버전 이후에는 createReadStream과 createWriteStream을 pipe하지 않아도 파일 복사가 가능하다.

```js
const fs = require('fs').promises;

// fs.copyFile('복사할 파일', '복사될 경로', 콜백 함수)
fs.copyFile('readme4.txt', 'writeme4.txt')
    .then(() => {
        console.log('복사 완료');
    })
    .catch((error) => {
        console.error(error);
    });
```

- watch 메서드 : 파일/폴더의 변경 사항 감시

```js
const fs = require('fs');

fs.watch('./target.txt', (eventType, filename) => {
    console.log(eventType, filename);
});

// 위 코드를 실행후 target.txt의 내용 또는 파일 변을 변경하면 터미널 console에 표기

// 내용 변경
// change target.txt
// change target.txt
// change 이벤트가 2번씩 발생 하기도 하므로 실무에서 사용할 때 주의해야한다.

// 폴더 변경 및 삭제
// rename change_target.txt
// rename 이벤트 이후 더 이상 watch가 수행 되지않는다. 
```