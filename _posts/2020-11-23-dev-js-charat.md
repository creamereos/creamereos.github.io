---  
layout: post
title: charAt(index)
categories: dev
tags: js
comments: true
---

문자열에서 (인자)로 주어진 값에 해당하는 문자를 리턴한다.

- syntax

```javascript
charAt(index)
```

- 예시 코드

```javascript

const str = '일이삼사오육칠팔구'
str.charAt(0); 
// 일

str.charAt(1);
// 이

str.charAt(str.length-1);
// 구 (마지막 문자 가져오기)

str.charAt(1000) == ''); 
// true
```

- charAt은 문자열에서만 사용 가능하다.

```javascript
const num = 12345789
str.charAt(str.length-1);
// 타입 에러 : Uncaught TypeError: a.charAt is not a function

const str = '12345789'
str.charAt(str.length-1);
// 9 (마지막 문자 가져오기)
```
