---  
layout: post
title: isNaN
categories: dev
tags: javascript
comments: true
---

- NaN : Not A Number의 약자. 즉 숫자가 아닌 것을 뜻한다.

```javascript
let A = 100 / "50";
// A = 2
// string "50"을 number 50으로 변환해서 계산

let B = 100 / "오십";
// A = NaN
```

- isNaN() 메소드 : NaN인지 아닌지를 체크하는 메소드. 

```javascript
let A = 100 / "50";
isNaN(A)
// false

let B = 100 / "오십";
isNaN(B)
// true
```

- ex code : if 숫자가 아니라면 

```javascript
C = '문자열'

if (Number.isNaN(C)){
    코드 실행
}
// 만약 C가 NaN이 아니면 true이므로 코드가 실행 됨.

D = 1

if (!Number.isNaN(D)){
    코드 실행
}
// 만약 D가 숫자이면 NaN이 아니므로 false. 거기에 부정 연산(!)을 붙여 true로 변경하여 실행.
```