---  
layout: post
title: nodeJS - multer
categories: dev
tags: node
comments: true
---

multer : 이미지, 동영상 등을 비롯한 여러가지 파일들을 멀티파트 형식으로 업로드할 때 사용하는 미들웨어.

멀티파트 형식 : enctype이 multipart/form-data인 폼을 통해 업로드하는 데이터 형식

```
$ npm i multer
```

upload 폴더가 꼭 존재해야한다. 없다면 직접 만들어주거나 
fs 모듈을 사용해서 서버를 시작할 때 생성

