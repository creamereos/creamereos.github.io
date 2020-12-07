---  
layout: post
title: nodeJS - 패키지 배포하기
categories: dev
tags: node
comments: true
---

- [npm 계정 만들기](https://www.npmjs.com){: target = "_blank"}

- 패키지 배포

```
$ npm publish
```

원하는 패키지 이름이 사용중이라면 package.json의 name을 바꿔야한다. (중복된 이름 불가)

- 패키지 이름이 존재하는지 확인하기

```
$ npm info 패키지 이름
```

- 패키지 삭제

```
$ npm unpublish 패키지이름 --force
```