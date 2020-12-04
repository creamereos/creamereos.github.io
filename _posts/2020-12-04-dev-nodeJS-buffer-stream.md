---  
layout: post
title: nodeJS - 버퍼와 스트림
categories: dev
tags: nodeJS
comments: true
---

readFile 등에서 받아온 data를 data.toString()으로 변환하는 이유는 data가 buffer이기 때문이다.

파일을 읽거나 쓰는 방식은 크게 Buffer와 stream을 이용하는 방식이 있다.

- 버퍼링(영상 로딩) : 영상을 재생할 수 있을 때 까지 데이터를 모으는 동작
- 스트리밍(영상 실시간 송출) : 영상 데이터를 조금씩 전송하는 동작.

노드의 버퍼와 스트림도 비슷한 개념이다. 노드는 파일을 읽을 때 메모리에 파일 크기 만큼 공간을 마련해두며 파일 데이터를 메모리에 저장한 뒤 사용자가 조작할 수 있도록 한다. 이때 **메모리에 저장된 데이터가 버퍼(Buffer)**이다.

**[Buffer 객체의 method 종류]**

- Buffer.from(문자열) : 문자열을 buffer로 바꿀 수 있다.

```js
const buffer = Buffer.from('저를 버퍼로 바꿔보세요');

// '저를 버퍼로 바꿔보세요'를 buffer로 변환
console.log('from():', buffer);
// from(): <Buffer ec a0 80 eb a5 bc 20 eb b2 84 ed 8d bc eb a1 9c 20 eb b0 94 ea bf 94 eb b3 b4 ec 84 b8 ec 9a 94>
```

- Buffer.from(문자열).length : Buffer의 크기 (단위: bytes)

```js
// buffer의 길이
console.log('length:', buffer.length);
// length: 32
```

- Buffer.from.(문자열)toString() : Buffer를 String으로 변환

```js
// buffer를 string으로 변환
console.log('toString():', buffer.toString());
// toString(): 저를 버퍼로 바꿔보세요
```

- Buffer.from(문자열).concat(배열들) : 배열 안에 든 버퍼들을 하나로 합친다.

```js
const array = [Buffer.from('띄엄 '), Buffer.from('띄엄 '), Buffer.from('띄어쓰기')];
const buffer2 = Buffer.concat(array);
console.log('concat():', buffer2.toString());
// concat(): 띄엄 띄엄 띄어쓰기
```

- Buffer.alloc(바이트 숫자) : 빈 버퍼를 생성. 바이트를 인수()에 넣으면 해당 크기의 버퍼가 생성된다.

```js
const buffer3 = Buffer.alloc(5);
console.log('alloc():', buffer3);
// alloc(): <Buffer 00 00 00 00 00>
```

readFile 방식의 버퍼가 편리하기는 하지만 **문제점**도 있다. 만약 용량이 100mb인 파일이 있으면 읽을 때 메모리에 100mb의 버퍼를 만들어야 한다. 이 작업을 동시에 10개만 해도 1GB에 달하는 메모리가 사용된다. 특히 서버처럼 몇명이 이용할지 모르는 환경에서 **메모리 문제**가 발생할 수 있다.

또한 **모든 내용을 버퍼에 다 쓴 후에야 다음 동작으로 넘어가므로** 파일 읽기, 압축, 파일 쓰기 등의 조작을 연속으로 할 때 **매번 전체 용량을 버퍼로 처리해야 다음 단계**로 나아갈 수 있다.

그래서 **버퍼의 크기를 작게 만든 후 여러 번으로 보내는 방식(스트림)**이 등장했다. (예를 들어 100MB 파일은 1MB 파일을 만들어 백번에 걸쳐서 나눠 보내는 것)

- createReadStream : 스트림으로 파일 읽기

```js
const fs = require('fs');

// require('fs').createReadStream('파일의 경로', 옵션 객체) : 읽기 스트림 만들기.
// highWaterMark라는 옵션은 버퍼의 크기(바이트 단위)를 정할 수 있는 옵션. 
// 16B로 설정
// 기본값은 64KB이지만, 여러번 나눠서 보내는 모습을 보여주기 위해 16B로 낮춤.
const readStream = fs.createReadStream('./readme3.txt', { highWaterMark: 16 });
const data = [];

// readStream은 이벤트 리스너(.on)를 붙여서 사용한다. 보통 data, end, error 이벤트를 사용한다.

// 파일 읽기 시작 이벤트 리스너 사용 : reatream.on('data')
readStream.on('data', (chunk) => {
    data.push(chunk);
    console.log('data:', chunk, chunk.length);
    // data: <Buffer ec 8a a4 ed 8a b8 eb a6 bc 20 3a 20 ec a0 80 eb> 16
    // data: <Buffer 8a 94 20 ec a1 b0 ea b8 88 ec 94 a9 20 ec a1 b0> 16
    // data: <Buffer ea b8 88 ec 94 a9 20 eb 82 98 eb 88 a0 ec 84 9c> 16
    // data: <Buffer 20 ec a0 84 eb 8b ac eb 90 a9 eb 8b 88 eb 8b a4> 16
    // data: <Buffer 2e 20 eb 82 98 eb 88 a0 ec a7 84 20 ec a1 b0 ea> 16
    // data: <Buffer b0 81 ec 9d 80 20 63 68 75 6e 6b eb 9d bc ea b3> 16
    // data: <Buffer a0 20 eb b6 80 eb a6 85 eb 8b 88 eb 8b a4 2e> 15
});

// 파일 읽기 완료 이벤트 리스너 사용 : reatream.on('end')
readStream.on('end', () => {
    console.log('end:', Buffer.concat(data).toString());
    // end: 스트림 : 저는 조금씩 조금씩 나눠서 전달됩니다. 나눠진 조각은 chunk라고 부릅니다.
});

// 에러 발생 시 이벤트 리스너 사용 : reatream.on('error') 
readStream.on('error', (err) => {
    console.log('error:', err);
});
```

- createWriteStream : 스트림으로 파일 쓰기

```js
const fs = require('fs');

// createWriteStream으로 쓰기 스트림 만들기
// require('fs').createWriteStream('만들 파일 명과 경로')
const writeStream = fs.createWriteStream('./writeme2.txt');

// finish 이벤트 리스너 : writeStream.on('finish') - 스트림으로 파일 쓰기가 종료된 콜백 함수 호출
// 3.스트림으로 파일 쓰기가 writeStream.on 이벤트리스너로 finish 되면 콜백 함수로 '파일 쓰기 완료'를 콘솔에 적어라.
writeStream.on('finish', () => {
    console.log('파일 쓰기 완료');
    // 파일 쓰기 완료
});

// 1.스트림으로 파일 쓰기.
// require('fs').createWriteStream('만들 파일 명과 경로').write(문자열)
writeStream.write('이 글을 씁니다. \n');
writeStream.write('이 글을 한번 더 씁니다.');

// require('fs').createWriteStream('만들 파일 명과 경로').end()
// 2.스트림으로 파일 쓰기 종료. 직후 3번의 finish 이벤트가 발생한다.
writeStream.end();
```

createReadStream으로 파일을 읽고 그 스트림을 전달받아 createWriteStream으로 파일을 쓸 수도 있다. (파일 복사와 비슷)
**스트림끼리 연결**하는 것을 **'파이핑'**이라고 표현한다. 

readme4.txt (복사할 파일)

```txt
저를 writeme3.txt로 보내주세요.
```

pipe.js

```js
// fs 모듈 불러오기
const fs = require('fs')

// 파일 읽기
const readStream = fs.createReadStream('readme4.txt');

// 파일 쓰기
const writeStream = fs.createWriteStream('writeme3.txt');

// 읽은파일.pipe(쓴 파일)
readStream.pipe(writeStream);
```

```console
$ node pipe
```

writeme3.txt (복사해서 새롭게 생성된 파일)

```txt
저를 writeme3.txt로 보내주세요.
```

pipe는 스트림 사이에 여러 번 연결 할 수 있다. 


- gzip : 파일을 읽은 후 gzip 방식으로 압축

```js
// zlib : 파일을 압축하는 모듈
const zlib = require('zlib');
const fs = require('fs');

const readStream = fs.createReadStream('./readme4.txt');
// zlib의 메소드 createGzip이라는 메서드가 스트림을 지원
const zlibStream = zlib.createGzip();
const writeStream = fs.createWriteStream('./readme4.txt.gz');

// zlib의 메소드 createGzip이라는 메서드가 스트림을 지원하므로 readStream과 writeSteam 중간에서 파이핑 가능.
readStream.pipe(zlibStream).pipe(writeStream);
// 버퍼 데이터가 전달되다가 gzip 압축을 거친 후 파일로 작성된다.

// readme4.txt.gz 파일 생성. 압축된 파일이라 파일을 읽지는 못한다.
```

**[readFile 메서드 vs createReadSteam 메서드]**

메모리 사용량 비교 : readFile 메서드 vs createReadSteam 메서드

createBigFile.js (1GB 용량의 텍스트 파일을 만드는 코드)

```js
const fs = require('fs');
const file = fs.createWriteStream('./big.txt');

for(let i = 0; i <= 10000000; i++) {
    file.write('안녕하세요. 엄청나게 큰 파일을 만들어 볼 것입니다.\n');
}

file.end();
```

- buffer(readFile) 메모리 사용

buffer-memory.js 

```js
const fs = require('fs');

// 기존 메모리 사용량
console.log('before: ', process.memoryUsage().rss);
// before:  18722816

// 1GB 파일 읽기
const data1 = fs.readFileSync('./big.txt');

// 1GB 파일 쓰기
fs.writeFileSync('./big2.txt', data1);
console.log('buffer:', process.memoryUsage().rss);
// buffer:  91885568

```

처음에 18MB(18718720)였던 메모리 용량이 순식간에 1GB(를 넘었다. 1GB 용량의 파일을 복사하기 위해 메모리에 파일을 모두 올려둔 후 wirteFileSync를 수행 했기 때문이다.

- stream(createReadStream) 메모리 사용

```js
const fs = require('fs');

console.log('before: ', process.memoryUsage().rss);
// before:  18685952

const readstream = fs.createReadStream('./big.txt');
const writeStream = fs.createWriteStream('./big3.txt')

readstream.pipe(writeStream);
readstream.on('end', () => {
    console.log('stream: ', process.memoryUsage().rss);
    // stream:  73973760
});
```

stream을 사용하여 파일 복사 시 메모리를 적게 차지 한다. 이렇듯 stream을 사용하면 효과적으로 데이터를 전송 할 수 있다. 동영상 같은 큰 파일들을 전송 할 때 stream을 사용하는 이유다.