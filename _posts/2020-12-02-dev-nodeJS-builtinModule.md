---  
layout: post
title: nodeJS - built-in module (os / path)
categories: dev
tags: nodeJS
comments: true
---

노드는 웹 브라우저에서 사용 되는 javascript보다 더 많은 기능 제공한다. 운영 체제 정보 접근, 클라이언트가 요청한 주소에 대한 정보 가져오기 등 노드는 이러한 기능을 하는 모듈을 제공한다.

- OS 모듈 : 운영체제 정보 가져오기

os.js

```javascript
// 모듈 불러오기
const os = require('os');

// os 아키텍쳐 정보
console.log(os.arch());

// os 플랫폼 정보
console.log(os.platform());

// os 종류
console.log(os.type());

// os 부팅 이후 흐른 시간(초) (process.uptime()은 노드의 실행 시간)
console.log(os.uptime());

// 컴퓨터의 이름
console.log(os.hostname());

// 홈 디렉토리 경로
console.log(os.homedir());

// 임시 파일 저장 경로
console.log(os.temdir());

// CPU 정보
console.log(os.cpus());

// CPU 개수
// 노드에서 싱글 스레드 프로그래밍을 하면 코어가 몇개든 상관없이 대부분의 경우 코어를 하나만 사용. cluster 모듈을 사용하여 코어 개수에 맞춰서 프로세스를 늘릴 수 있다.
console.log(os.cpus.length())

// 사용 가능한 메모리(RAM)
console.log(os.freemem())

// 전체 메모리(RAM)
console.log(os.totalmem())
```

- path : 폴더와 파일 경로를 쉽게 조작하도록 도와주는 모듈. 
path 모듈이 필요한 이유 중 하나는 os 별로 경로 구분자가 다름 (윈도우 : \ (역슬래시), 맥: / (슬래시))

```javascript
// 모듈 불러오기
const path = require('path');

// 경로의 구분자 
console.log(path.sep);
// 맥: / , 윈도우: \

// 환경 변수의 구분자 
console.log(path.delimiter);
// 맥: 콜론(:), 윈도우: 세미콜론(;)

// 파일이 위치한 경로
console.log(path.dirname(string));
// desktop/usr/creamer

// 파일의 확장자
console.log(path.extname(string));
// .js

// 확장자를 포함한 파일의 이름.
console.log(path.basename(string));
// module.js 

// 파일의 이름만 표시. basename의 두번째 인수로 파일의 확장자를 넣으면 된다.
console.log(path.basename(string, path.extname(string)));
// module

// 파일의 경로를 root, dir, base, ext, name으로 나눠서 표시
console.log(path.parse(string));

// path.parse()한 객체를 파일 경로로 합친다.
console.log(path.format({객체}));

// \ 또는 / 를 여러번 사용 했거나 혼용 했을때 정상 경로로 변환
console.log(path.nomalize('desktop//creamer\\'));

// 절대 경로인지 상대 경로(false) 인지 파악.
console.log(path.isAbsolut('경로'));
// 절대 경로 : true, 상대 경로 : false

// 기준 경로에서 비교 경로로 가는 방법을 알려준다.
console.log(path.relative('기준경로', '비교경로'));
// ../../..

// 여러 인수를 넣으면 하나의 경로로 합친다. 상대 경로인 ..(부모 디렉토리)와 .(현재 디렉토리)도 알아서 처리.
console.log(path.join(___dirname, '비교경로'));

// path.join과 비슷하지만 차이점이 있다.
console.log(path.resolve(___dirname, '비교경로'));
```

[path.join vs path.resolve]

- path.join :  /(슬래시)를 만났을때 상대 경로로 처리

```javascript
path.join('/a', '/b', 'c'); 
// 결과 : /a/b/c/ 
```

- path.resolve :  /(슬래시)를 만났을때 절대 경로를 인식해서 앞의 경로 무시

```javascript
path.resolve('/a', '/b', 'c'); 
// 결과 : /b/c/ 
```

[/(슬래시) vs //(쌍슬래시)]

- /(슬래시) : 기본적으로 경로는 슬래시 하나만 사용

- //(쌍슬래시) : 자바스크립트 문자열에서 /가 특수 문자 이므로 //를 사용
ex : desktop/node에서 /n은 자바스크립트 문자열에서 줄바꿈이라는 뜻이므로 오류가 발생 할 수 있으므로 쌍 슬래시를 사용. desktop//node

 
[상대 경로 vs 절대 경로]

- 상대 경로 : **현재 파일이 기준** 현재 파일과 같은 경로면 점 하나(.) 한단계 상위 폴더면 점 두개(..)

- 절대 경로 : **루트 폴더(/)나 노드 프로세스**가 실행되는 위치가 **기준**