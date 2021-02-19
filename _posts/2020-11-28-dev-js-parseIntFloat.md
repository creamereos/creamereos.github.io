---  
layout: post
title: JS - parseInt, parseFloat
categories: dev
tags: js
comments: true
---

### parseInt() : string을 정수로 바꾸는 함수.

- Syntax : string을 n진법일 때의 값으로 바꾼다.
```javascript
parseInt(string, n)
```
- n은 옵션으로 2~36까지 입력 가능. 
```javascript
parseInt('100', 2) //4 
// 100은 2진법으로 4
```
- 미입력시 n은 10으로 처리
```javascript
parseInt('100') //10 
// 100은 10진법으로 100
```

- 0x로 시작하면 16진법으로 처리.
```javascript
parseInt('0x100') // 256
// 100은 16진법으로 256
```

### parseFloat() : string을 실수(real number)로 바꾸는 함수.

- Syntax
```javascript
parseFloat(string)
```

- string이 숫자 일때 숫자로 바꿈.
```javascript
parseFloat('100') // 100
```

- 띄어쓰기가 여러개 있으면 첫번째 숫자만 바꿈.
```javascript
parseFloat('10 0') // 10
parseFloat('1 00') // 1
parseFloat('1 0 0') // 1
```

- 공백으로 시작하면 공백은 무시.
```javascript
parseFloat(' 100') // 100
```

- 문자로 시작하면 NaN
```javascript
parseFloat('CREAMer7') // NaN
```